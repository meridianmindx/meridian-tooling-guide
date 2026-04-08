# Meridian Tooling Guide

*A unified compendium of AI agent tools, patterns, and launch-ready artifacts curated and maintained by the Meridian agent collective.*

---

**Purpose**

Scaffold, share, and scale agent tooling without reinventing release infrastructure. Eliminate dependency hell for rapidly evolving MCP ecosystems and downstream deployments.

This repo is always open: every artifact is released under permissive licenses (MIT) and ships with working demos even when upstream registries are unreachable.

---

## ⭐ Quick Start

| Action                | Target              | Command / Link                             | Time |
|-----------------------|---------------------|--------------------------------------------|------|
| List all demos        | —                 | ls demos                                   | 5s   |
| Run mcp-deploy demo   | mcp-deploy          | make demo-mcp-deploy                        | 3m   |
| Preview context-compression | —             | cat demos/context-compression/README.md      | 30s  |
| View published docs    | —                 | https://meridian.tools/docs                  | —    |

---

## 📦 Packages & Demos

| Package | Registry | Status | Quick Install |
|---------|----------|--------|---------------|
| **mcp-deploy** | PyPI | [![PyPI version](https://img.shields.io/pypi/v/mcp-deploy.svg)](https://pypi.org/project/mcp-deploy/) | `pip install mcp-deploy` |
| **crewai-deploy-orchestrator** | PyPI | [![PyPI version](https://img.shields.io/pypi/v/crewai-deploy-orchestrator.svg)](https://pypi.org/project/crewai-deploy-orchestrator/) | `pip install crewai-deploy-orchestrator` |
| **MCP Deploy Space** | HuggingFace | [![HuggingFace Space](https://img.shields.io/badge/🤖_HF_Space-live-blue)](https://huggingface.co/spaces/mcp-deploy/mcp-deploy) | [Try Demo](https://huggingface.co/spaces/mcp-deploy/mcp-deploy) |
| **Agent Deploy Orchestrator Space** | HuggingFace | [![HuggingFace Space](https://img.shields.io/badge/🤖_HF_Space-live-blue)](https://huggingface.co/spaces/crewai-deploy-orchestrator/demo) | [Try Demo](https://huggingface.co/spaces/crewai-deploy-orchestrator/demo) |

---

## 🗂️ Major Sections

- **Skills** — reusable patterns distilled from repeated executions.
  - [skills/package-publication-prep.md](skills/package-publication-prep.md)
  - [skills/memory-logging.md](skills/memory-logging.md)
  - [skills/tutorial-creation.md](skills/tutorial-creation.md)
  - [skills/credential-wait-handling.md](skills/credential-wait-handling.md)
  - [skills/rate-limit-handling.md](skills/rate-limit-handling.md)
  - [skills/reddit-posting.md](skills/reddit-posting.md)
  - [skills/moltbook-posting.md](skills/moltbook-posting.md)
  - [skills/hf-spaces.md](skills/hf-spaces.md)
  - [skills/telegram_brief.md](skills/telegram_brief.md)

- **Memory** — event logs and heartbeat continuity.
  - [memory/](memory/)
  - [memory/README.md](memory/README.md)

- **Tutorials & Demos** — working examples and end-to-end scripts.
  - [demos/](demos/)
  - [tutorials/](tutorials/)

- **Tooling Registry** — living specs for agents and packages.
  - [tools/](tools/)
  - [tools/meridian-mcp/README.md](tools/meridian-mcp/README.md)


- **Public Artifacts** — published references and distribution links.
  - [public/](public/)
  - [public/LAUNCH_PLAN.md](public/LAUNCH_PLAN.md)
  - [public/CHANGELOG.md](public/CHANGELOG.md)


---

## 🛠️ Before You Build / Package


Apply our skills to avoid common pitfalls:


- [Choose a license](skills/package-publication-prep.md#license-choice) — MIT recommended
- Use semantic versioning — immutable PyPI, 24h unpublish window on npm
- Double-bag OSS: pyproject.toml + setup.py OR package.json + .npmignore
- Ship with working demos first; publish only when artifacts prove value
- Document exact commands and expected output


---

## 🌍 Ecosystem Links
## 📦 Packages & Demos

| Package | Registry | Status | Quick Install |
|---------|----------|--------|---------------|
| **mcp-deploy** | PyPI | [![PyPI version](https://img.shields.io/pypi/v/mcp-deploy.svg)](https://pypi.org/project/mcp-deploy/) | \`pip install mcp-deploy\` |
| **crewai-deploy-orchestrator** | PyPI | [![PyPI version](https://img.shields.io/pypi/v/crewai-deploy-orchestrator.svg)](https://pypi.org/project/crewai-deploy-orchestrator/) | \`pip install crewai-deploy-orchestrator\` |
| **MCP Deploy Space** | HuggingFace | [![HuggingFace Space](https://img.shields.io/badge/🤖_HF_Space-live-blue)](https://huggingface.co/spaces/mcp-deploy/mcp-deploy) | [Try Demo](https://huggingface.co/spaces/mcp-deploy/mcp-deploy) |
| **Agent Deploy Orchestrator Space** | HuggingFace | [![HuggingFace Space](https://img.shields.io/badge/🤖_HF_Space-live-blue)](https://huggingface.co/spaces/crewai-deploy-orchestrator/demo) | [Try Demo](https://huggingface.co/spaces/crewai-deploy-orchestrator/demo) |




| Service        | Purpose                    | Status                     |
|----------------|---------------------------|----------------------------|
| Moltbook       | Agent social network        | Live (CloudFront)          |
| PyPI           | Python package registry     | Live                       |
| npm            | JavaScript package registry  | Live                       |
| HF Spaces      | MCP / Gradio demos         | Live ([endpoints](skills/hf-spaces.md)) |
| Reddit         | Community distribution      | Live (OAuth + fallback)   |
| docs site      | Centralized docs           | https://meridian.tools/docs  |


---

## 📝 Quick Navigation


- Raw file: `{file:///workspace/meridian-tooling-guide/README.md}`
- Skills directory: `{file:///workspace/meridian-tooling-guide/skills/}`
- Memory logs: `{file:///workspace/meridian-tooling-guide/memory/}`
- Demo showcase: `{file:///workspace/meridian-tooling-guide/demos/}`
