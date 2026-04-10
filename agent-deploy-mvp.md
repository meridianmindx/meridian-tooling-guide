# Agent Deployment MVP Guide

A practical guide to deploying AI agents (CrewAI, LangGraph, autogen) with minimal infrastructure using MCP-based tooling.

---

## What You'll Build

A production-ready agent system that:
- Runs 24/7 with health monitoring
- Scales from 1 to many concurrent crews
- Supports hot-swapping without downtime
- Logs all actions for debugging

---

## Before You Start

**Time**: 5 minutes  
**Prerequisites**:
- Docker installed
- Python 3.10+ (for building agents)
- OpenAI/Anthropic API key

**What You Get**:
- Docker Compose setup
- Health check endpoints
- Log aggregation
- Graceful shutdown handling

---

## Step 1: Create Your Agent

CrewAI example (minimal):

```python
# crew.py
from crewai import Agent, Task, Crew, Process

researcher = Agent(
    role='Senior Researcher',
    goal='Discover breakthrough AI trends',
    backstory="You're a brilliant researcher who finds patterns others miss.",
    verbose=True
)

writer = Agent(
    role='Content Writer',
    goal='Write compelling blog posts',
    backstory="You turn complex research into engaging stories.",
    verbose=True
)

task = Task(
    description='Research latest AI agent frameworks and write a summary',
    agent=researcher,
    expected_output='Markdown report with insights'
)

crew = Crew(
    agents=[researcher, writer],
    tasks=[task],
    process=Process.sequential
)

if __name__ == "__main__":
    result = crew.kickoff()
    print(result)
```

---

## Step 2: Generate Deployment Config

Using `meridian-mcp-deploy`:

```bash
# Install meridian-mcp-deploy
pip install meridian-mcp-deploy

# Generate docker-compose.yml from your agent
meridian-mcp-deploy --server crew.py --output ./deploy --target docker

# Preview generated files
ls deploy/
# docker-compose.yml  Dockerfile  README.md  healthcheck.sh
```

**Output includes**:
- `Dockerfile` with Python + dependencies + health check
- `docker-compose.yml` with logging, restart policy, resource limits
- `README.md` with exact run commands
- `healthcheck.sh` for container liveness

---

## Step 3: Deploy Locally

```bash
cd deploy
docker-compose up -d

# View logs
docker-compose logs -f agent

# Check health
curl http://localhost:8080/health
# {"status":"healthy","timestamp":"..."}

# Stop
docker-compose down
```

---

## Step 4: Deploy to Cloud

### Option A: Same Docker Compose (any host)
Upload the `deploy/` directory to any server with Docker:
```bash
scp -r deploy/ user@cloud-server:/opt/agent-deploy
ssh user@cloud-server "cd /opt/agent-deploy && docker-compose up -d"
```

### Option B: Kubernetes (with meridian-mcp-deploy --target kubernetes)
Generated manifests support:
- Deployments with rolling updates
- Services (ClusterIP/LoadBalancer)
- ConfigMaps/Secrets for API keys
- Horizontal Pod Autoscaler

```bash
# Generate k8s manifests
meridian-mcp-deploy --server crew.py --output k8s/ --target kubernetes

# Apply
kubectl apply -f k8s/
```

---

## Architecture

```
┌─────────────────┐
│   LLM API Key   │
│   (env var)     │
└────────┬────────┘
         │
    ┌────▼────┐
    │ Agent   │
    │ Process │
    └────┬────┘
         │
    ┌────▼─────────────────────┐
    │ Docker Container         │
    │ - Python env             │
    │ - Dependencies           │
    │ - Health check endpoint │
    └────┬─────────────────────┘
         │
    ┌────▼──────────────┐
    │ Docker Compose    │
    │ - Logging driver  │
    │ - Restart policy  │
    │ - Resource limits │
    └────┬──────────────┘
         │
    ┌────▼────────────┐
    │ Host/Cloud      │
    │ (any Docker)    │
    └─────────────────┘
```

---

## Use Cases

### 1. Research Automation
- Web research + summarization
- Competitive analysis
- Literature review

### 2. Content Pipeline
- Blog post generation
- Social media content
- Newsletter curation

### 3. Code Analysis
- Security scanning
- Dependency updates
- Documentation generation

### 4. Customer Support
- Ticket triage
- FAQ generation
- Escalation routing

---

## Advanced: Multi-Agent Orchestration

Use `meridian-crewai-deploy-orchestrator` for coordinated multi-crew deployments:

```python
# orchestrator.py
from crewai_deploy_orchestrator import Orchestrator

orch = Orchestrator(
    crews=["research-crew.yaml", "analysis-crew.yaml"],
    schedule="0 */6 * * *"  # Every 6 hours
)

orch.run()
```

Deploy with:
```bash
meridian-mcp-deploy --server orchestrator.py --output ./orchestrator-deploy/
```

---

## Monitoring & Observability

The generated setup includes:

- **Health checks**: `/health` endpoint responds with JSON status
- **Logs**: stdout/stderr captured by Docker; view with `docker-compose logs`
- **Metrics**: Optional Prometheus endpoint if `--metrics` flag used
- **Restart policy**: `unless-stopped` ensures recovery from crashes

For production:
- Ship logs to ELK/Loki/Datadog
- Set up alerts on health check failures
- Use Docker secrets for credential management

---

## Troubleshooting

### Container exits immediately
**Cause**: Agent crashed on startup.  
**Fix**: Check logs: `docker-compose logs agent`. Common issues:
- Missing API key environment variable
- Import errors in agent code
- Port already in use

### Health check fails
**Cause**: Agent not ready within 30s.  
**Fix**: Increase `healthcheck.timeout` in generated `docker-compose.yml` or fix agent initialization.

### Out of memory
**Cause**: LLM model too large for container.  
**Fix**: Add resource limits: `mem_limit: 4g` in `docker-compose.yml`. Use smaller model or external inference API.

### can't pull image
**Cause**: Dockerfile build failed or base image unavailable.  
**Fix**: Build manually: `docker build -t my-agent .` and inspect errors. Verify Python version matches base image.

---

## Scaling Strategies

| Scale | Approach | When to Use |
|-------|----------|-------------|
| Single server | docker-compose with multiple services | Development, small workloads |
| Multi-service | Docker Swarm | Medium scale, need orchestration |
| Cloud-native | Kubernetes with HPA | Production, variable load |
| Serverless | meridian-mcp-deploy --target serverless | Sporadic, event-driven workloads |

---

## Security Checklist

- [ ] Store API keys in Docker secrets or env files (not in code)
- [ ] Run containers as non-root user
- [ ] Limit container capabilities (`--cap-drop ALL`)
- [ ] Use read-only filesystem where possible
- [ ] Scan Docker images for vulnerabilities
- [ ] Update base images regularly

---

## Next Steps

1. **Customize**: Add your domain-specific tools to the agent
2. **Optimize**: Fine-tune prompts and task decomposition
3. **Monitor**: Implement alerting on failure rates
4. **Scale**: Move from docker-compose to K8s when needed
5. **Share**: Publish your agent as an MCP server for others

---

## Related

- [mcp-overview.md](mcp-overview.md) — MCP fundamentals
- [skills/moltbook-posting.md](../skills/moltbook-posting.md) — Promote your agent
- [demo-showcase/README.md](../demo-showcase/README.md) — Working examples
- [skills/package-publication-prep.md](../skills/package-publication-prep.md) — Package for PyPI/npm

---

## Related

- **[mcp-overview.md](mcp-overview.md)** — MCP fundamentals and tooling
- **[README.md](README.md)** — Main guide index
- **[skills/memory-logging.md](../skills/memory-logging.md)** — Production logging patterns
- **[skills/credential-wait-handling.md](../skills/credential-wait-handling.md)** — Handling credential blockers
- **[demo-showcase/README.md](../demo-showcase/README.md)** — Working examples
- **[docs/agent-deploy-orchestrator/README.md](../docs/agent-deploy-orchestrator/README.md)** — Orchestrator reference
