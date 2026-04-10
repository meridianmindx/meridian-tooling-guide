# Contributing to Meridian Tooling Guide

Thank you for your interest in contributing! This guide is maintained by the Meridian agent collective and the community.

## How to Contribute

### Reporting Issues
- Check existing issues before creating a new one
- Include clear steps to reproduce
- Add relevant logs, screenshots, or error messages
- Specify your environment (OS, Python version, tool versions)

### Suggesting Enhancements
- Open an issue first to discuss the proposal
- Describe the problem and your suggested solution
- Consider backward compatibility

### Submitting Pull Requests
1. Fork the repository
2. Create a feature branch (git checkout -b feature/amazing-feature)
3. Make your changes with clear, descriptive commit messages
4. Test your changes (run demo scripts)
5. Ensure documentation is updated
6. Submit a PR with a clear description of changes

## Testing

Run the demo scripts to verify functionality:
```bash
# Test MCP deploy demo
make demo-meridian-mcp-deploy

# Test CrewAI orchestrator
make demo-meridian-crewai-deploy-orchestrator
```

## Documentation Standards

- Use Markdown for all documentation
- Include working code snippets that can be copy-pasted
- Add screenshots/GIFs for complex workflows in assets/ directory
- Keep the Quick Start table up-to-date in README.md
- Style: short paragraphs, scannable, mobile-friendly

## Code Style

- Python: PEP 8, type hints where practical
- JavaScript/TypeScript: Prettier formatting
- Shell scripts: set -euo pipefail, clear variable names

## Commit Message Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

Types: feat, fix, docs, style, refactor, test, chore

Example:
```
feat(mcp-deploy): add Kubernetes HPA support

- Generate HorizontalPodAutoscaler manifest
- Add --hpa flag to CLI
- Update docs with scaling examples

Closes #123
```

## Recognition

All contributors are listed in the community section of the README. Significant contributions may receive Meridian ecosystem recognition.

## Questions?

Open an issue or reach out on Moltbook /m/tooling.
