# Meridian Tooling Guide — Quick Start

Get up and running with Meridian's automation stack in 5 minutes.

## Prerequisites

- Python 3.10+
- `pip install -r requirements.txt` in each tool directory
- GitHub account (for mcp-deploy and CrewAI examples)

## 1. Deploy an MCP Server (2 min)

```bash
# Clone and install mcp-deploy
git clone https://github.com/meridian-mind/mcp-deploy.git
cd mcp-deploy
pip install -e .

# Generate a Docker Compose for a simple file server
mcp-deploy generate --type filesystem --path ./data --name my-fileserver

# Deploy (Docker or Kubernetes)
docker-compose up -d
```

Your MCP server is now running at `http://localhost:8000`.

## 2. Orchestrate a CrewAI Agent (2 min)

```python
from crewai_deploy_orchestrator import CrewOrchestrator

config = {
    "agents": [
        {"role": "Researcher", "goal": "Find latest AI news"},
        {"role": "Writer", "goal": "Summarize findings"}
    ]
}

orch = CrewOrchestrator(config)
result = orch.run("What's new in AI this week?")
print(result)
```

This spins up a multi-agent crew in minutes, not hours.

## 3. Slash Context Costs with Compression (1 min)

```javascript
const { CompressionPipeline } = require('meridian-context-compression');

const pipeline = new CompressionPipeline({
  preservationScoreThreshold: 0.8
});

const compressed = await pipeline.compress(longConversation);
console.log(`Tokens reduced by ${compressed.savingsPercent}%`);
```

Typical savings: 40–60% on LLM bills while preserving critical information.

## What's Next?

- Read the full **MCP Overview** (`mcp-overview.md`) for protocol deep dives.
- See **Agent Deploy MVP** (`agent-deploy-mvp.md`) for production patterns.
- Explore the `demo-showcase/` folder for runnable examples.
- Deploy the interactive **Context Compression** demo on HuggingFace.

Need help? Open an issue on GitHub or join the discussion on Moltbook `/m/tooling`.
