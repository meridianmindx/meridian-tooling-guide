---
draft: true
---

## Scaling & Resilience Deep-Dive: agent-deploy-orchestrator

• Max pods per ns: 20 (dev), 100 (prod) — heap ≈120Mi / pod
• HPA triggers: cpu ≥50% for 5m or custom `/metrics` (Prometheus)
• Sidecar costs on pod: ≤8% cpu, ≤20Mi memory overhead; turn off with `--no-sidecars` flag

### Prometheus metrics (opt-in)
- All telemetry served on `:9090/metrics`; add Prometheus scrape config:
  ```yaml
  - job_name: agent-deploy-orchestrator
    scrape_interval: 30s
    static_configs:
      - targets: ["<namespace>.svc.cluster.local:9090"]
  ```

### Failure recovery pillar: initContainers
1. Pre-flight init: validate secrets + configmap (exit 0=OK, >0=retry with backoff)
2. Once per config: pull base containers (exit codes 0/41=OK, >41=permanent failure)
3. Config-apply init (exponential backoff)

Typical kubectl install retry pattern:
- `kubectl wait --for condition=Ready --timeout=5m pod -l app=agent-deploy-orchestrator` > if fail, runs init again after exponential backoff
- Automatic restart is controlled by `restartPolicy: Never` on init → pod stays pending until fixed.

### Artifact staging (backup without credentials)
Local outcomes stored in `/workspace/artifacts/agent-deploy-orchestrator/<ns>/<iso8601>/`:
- Pod spec YAML
- Resource usage logs (Prometheus scrape)
- Failure timestamps and exit codes (ansible events)
Daily rotation keeps only last 7 dumps.