# Operum

[![License: Proprietary](https://img.shields.io/badge/License-Proprietary-red.svg)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/operum-ai/operum?style=social)](https://github.com/operum-ai/operum/stargazers)
[![Works with Claude Code](https://img.shields.io/badge/Works%20with-Claude%20Code-blueviolet)](https://claude.ai/code)

> *"Labor omnia vincit"* — Work conquers all

**The complete multi-agent AI development team** — orchestrate 6 specialized Claude agents from a single desktop app.

Operum (Latin: *of works*, from *opus*) automates your development, marketing, and community workflows with a team of AI agents that coordinate through a label-driven pipeline — so a solo founder can ship like a full team.

## Download

Get the latest Operum desktop app:

| Platform | Download |
|----------|----------|
| Linux | [.deb](https://github.com/operum-ai/operum/releases/latest) · [AppImage](https://github.com/operum-ai/operum/releases/latest) |
| Windows | [.exe installer](https://github.com/operum-ai/operum/releases/latest) |
| macOS | [.dmg](https://github.com/operum-ai/operum/releases/latest) |

[View all releases &rarr;](https://github.com/operum-ai/operum/releases)

> **Requires** an active Claude Code (Max) subscription to run agents.

## How It Works

```mermaid
graph LR
    F([Founder]) --> PM[PM Agent]
    PM --> A[Architect]
    A -->|"ready-for-dev"| E[Engineer]
    E -->|"needs-testing"| T[Tester]
    T -->|"needs-review"| R[Review]
    R --> D[Done]
    T -.->|"FAIL"| E
```

Issues flow through the team automatically. Each agent owns specific pipeline stages and hands off to the next — while you stay in control as the founder.

| Agent | Role | What It Does |
|-------|------|-------------|
| **PM** | Orchestrator | Manages pipeline, delegates tasks, reports to founder |
| **Architect** | Technical Design | Reviews issues, provides implementation guidance |
| **Engineer** | Implementation | Writes code, creates PRs, runs tests |
| **Tester** | Quality Assurance | Tests PRs, approves or sends back for fixes |
| **Marketing** | Growth | SEO, content strategy, discoverability |
| **Community** | Support | Monitors channels, responds to users |

## Why Operum

- **A full team, solo** — six specialized agents cover architecture, code, QA, growth, and support.
- **You stay in control** — approve what matters; the agents handle the busywork.
- **Built on Claude** — powered by Anthropic's most capable models via Claude Code.
- **Desktop-native** — your code and credentials stay on your machine.

## Submit Feedback

### Bug Reports

Found an issue? [Create a bug report](https://github.com/operum-ai/operum/issues/new?template=bug_report.md&labels=bug)

Please include:
- Operum version (shown in the bottom-left of the app)
- Operating system
- Steps to reproduce
- Expected vs actual behavior

### Feature Requests

Have an idea? [Request a feature](https://github.com/operum-ai/operum/issues/new?template=feature_request.md&labels=enhancement)

### General Discussion

Questions or suggestions? [Start a discussion](https://github.com/operum-ai/operum/discussions)

## Community

- **Website**: [operum.ai](https://operum.ai)
- **Discord**: [Join our community](https://discord.gg/operum) *(coming soon)*
- **Documentation**: [operum.ai/docs](https://operum.ai/docs)

## Latin Glossary

| Term | Meaning | Context |
|------|---------|---------|
| *Operum* | Of works | Our name, from *opus* (work) |
| *Opus magnum* | Great work | What you'll build with Operum |
| *Ex nihilo* | From nothing | Start projects from scratch |
| *Carpe diem* | Seize the day | Ship faster with AI agents |
| *Per aspera ad astra* | Through hardships to the stars | The founder's journey |

---

© 2026 Operum AI. All rights reserved. *Made with dedication by solo creators, for solo creators.*
