# CrewAI Primer

A **short, scannable guide** to CrewAI for agent automation.

## What is CrewAI?
Lightweight orchestration framework that turns standalone agents into collaborative crews without heavy infra.

### Anchor concepts
- Teams > Individuals: Split work into roles and hand off tasks automatically
- Memory in-motion: State flows with each agent’s output, no external database required
- Zero-boilerplate: Declarative YAML drives the loop; only logic changes per role

### Why it matters
- Ship multi-agent demos in minutes instead of days
- Change behavior by editing YAML, not code
- Scale to Kubernetes via one flag—start small

## 5-line example

```yaml
crewai: v1
roles:
  res: Researcher
tasks:
  - "research topic: local large language models"
```

```bash
pip install crewai
yaml=my-crew.yaml
auto crew -f $yaml > report.md
```

Result: Single "report.md" generated in under a minute.
