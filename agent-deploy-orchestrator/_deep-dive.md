# Agent-Deploy-Orchestrator — Deep Dive

> Mature workload orchestrator for AI-agent tooling workloads on Kubernetes.

## Runtime & Resources
- **Scaling thresholds**
  - HA Fast lane: 10+ concurrent agent pods per cluster ⇒ scale control-plane pods to 3.
  - Volume load: 70 % disk-pressure on volume mount ⇒ increase PVC size via `kubectl patch`.
  - Operator load: >250 CRUD events/min ⇒ roll control-plane to 4 pods; 1000+ ⇒ add additional node.

## Delivering Observability
- **Sidecar topology:** Prometheus-compatible metrics exporter (JMX bridge)
  - Metrics emitted: workload_queue_size, agent_job_duration_seconds, pod_phase_gauge.
  - Logs shipped: JSON structured stdout with `trace_id` baked into every message; scrape via Prometheus scrape_config `pod_metrics/`.

## Stability & Recovery Patterns
- **Exponential backoff initContainers**
  ```yaml
  initContainers:
  - name: preflight-dep-wait
    image: alpine/curl
    command: ["sh","-c"]
    args:
      - for i in $(seq 1 10); do
          curl -sf http://dependency-service:8080/health || sleep $((2**$i));
        done
    # failures cascade safely to pod restartPolicy: Never
  ```

- **Backup & restore via side-effect artifacts**
  - Deferrable checkpoints: uploads session snapshots (serialized state + package whl) to pre-authenticated S3 bucket every 5 min window.
  - Rollback trigger: new major version rollout updates seed jobs with the previous artifact checksum; operator observes Prometheus metric `recovery_last_snapshot_age_seconds` for auto-trigger.
- **Emergency circuit breaker:** lock cluster to read-only if any control-plane pod is unavailable longer than 30 s; exposed via `/health/read-only` endpoint monitored by a PodDisruptionBudget.
