# DashClaw Agent

The definitive DashClaw expert — an AI agent that helps developers integrate, configure, troubleshoot, and build [DashClaw](https://github.com/ucsandman/DashClaw), the decision infrastructure and policy firewall for AI agents.

**Meta twist:** This agent governs itself on DashClaw. An agent for the agent governance platform, governed by the platform it represents.

## Run

```bash
# From GitHub
npx @open-gitagent/gitagent run -r git@github.com:ucsandman/dashclaw-agent.git

# With a prompt
npx @open-gitagent/gitagent run -r git@github.com:ucsandman/dashclaw-agent.git -p "Help me instrument my agent with DashClaw"

# Locally
git clone git@github.com:ucsandman/dashclaw-agent.git
cd dashclaw-agent
npx @open-gitagent/gitagent run
```

## Skills

| Skill | Category | Description |
|-------|----------|-------------|
| **instrument-agent** | Integration | Add DashClaw SDK to any agent — the 4-step governance loop (Guard → Record → Verify → Outcome) |
| **create-policies** | Configuration | Create guard policies: risk thresholds, action restrictions, approval gates, semantic guardrails |
| **setup-dashclaw** | Setup | Set up a DashClaw instance (local/cloud/demo), install CLI, configure Claude Code hooks |
| **manage-approvals** | Operations | Human-in-the-loop approval workflows via dashboard, CLI TUI, or SDK polling |
| **troubleshoot** | Debugging | Debug errors (401/403/429/503), signal issues, hook problems, and common gotchas |
| **build-dashclaw** | Development | Contribute to the DashClaw codebase — architecture, 12-step scaffold, tests, CI |
| **compliance-drift-evals** | Analytics | Compliance exports, drift detection, evaluations, scoring profiles, learning analytics |
| **register-on-dashclaw** | Meta | Register any agent (including this one) as a governed agent on DashClaw |

## Structure

```
dashclaw-agent/
├── agent.yaml                          # Agent manifest
├── SOUL.md                             # Identity, values, expertise
├── RULES.md                            # Hard constraints
├── README.md                           # This file
├── skills/
│   ├── instrument-agent/SKILL.md       # SDK integration guide
│   ├── create-policies/SKILL.md        # Guard policy creation
│   ├── setup-dashclaw/SKILL.md         # Instance + CLI + hooks setup
│   ├── manage-approvals/SKILL.md       # HITL approval workflows
│   ├── troubleshoot/SKILL.md           # Error diagnostics
│   ├── build-dashclaw/SKILL.md         # Contributing guide
│   ├── compliance-drift-evals/SKILL.md # Analytics capabilities
│   └── register-on-dashclaw/SKILL.md   # Agent registration (meta!)
└── knowledge/
    ├── index.yaml                      # Knowledge index
    ├── api-reference.md                # Core + extension API routes
    ├── sdk-reference.md                # V2 SDK (5-method surface)
    ├── legacy-sdk-reference.md         # V1 SDK (187 methods, 31 categories)
    └── architecture.md                 # System architecture & tech stack
```

## What is DashClaw?

DashClaw is decision infrastructure for AI agents. It sits between agents and external systems to:

- **Intercept** risky agent actions before execution
- **Enforce** governance policies (risk thresholds, approval gates, guardrails)
- **Require** human approval for sensitive operations
- **Record** verifiable evidence of every decision

Not an observability tool. Not an agent platform. The control plane for agent governance.

## Links

- [DashClaw](https://github.com/ucsandman/DashClaw) — The platform
- [gitagent](https://github.com/open-gitagent/gitagent) — The agent framework
