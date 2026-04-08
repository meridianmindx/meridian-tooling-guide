# Deep Dive: agent-deploy orchestrator

## Pod Scaling Thresholds
- **CPU** → 80% avg over 30s (horizontal-pod-autoscaler target)
- **Memory** → 70% of request (avoid evictions)
- **Pods** → Min 2 steady-state; Max 10; Burst to 15 via HPA when p99(request latency) > 300ms

## Sidecar Side-Effects
- **Logs** → JSON to stdout → Promtail → Loki (query: `{job="agent-deploy-orchestrator"}`)
- **Metrics** → `/metrics` (Prometheus Exposition) every 5s
  - `agent_deploy_duration_seconds` (histogram)
  - `agent_deploy_created_total` (counter, labels: status)
  - `agent_deploy_failures_total` (counter, labels: reason)

## Failure-Recovery Patterns

### 1. initContainer with Exponential Backoff
```yaml
initContainers:
- name: init-agent-connect
  image: alpine/curl:latest
  command: ["sh", "-c"]
  args:
    - set -e
      for i in $(seq 0 4); do
        curl -fs http://orchestrator-api:3000/health && exit 0
        sleep $((2 ** i))
      done
      echo "initContainer failed after 5 retries"
      exit 1
  resources:
    limits:
      cpu: 50m
      memory: 64Mi
```

### 2. Backup Artifact Staging
On failure:
- Copy `/workspace/artifacts/*.tgz` to S3 bucket `s3://meridian-backups/agent-deploy/<build_id>/` (retain 7 days)
- Attach `backup-id: <uuid>` label to pod via downward API
- Trigger alert: Slack `#deploy-alerts` and PagerDuty `agent-deploy-failure`
