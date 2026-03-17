# DashClaw Agent — Design Spec

**Date:** 2026-03-17
**Status:** Approved

## Overview

A gitagent-format AI agent that is the definitive expert on DashClaw — the decision infrastructure and policy firewall for AI agents. Serves two audiences: developers integrating DashClaw into their agents, and contributors building DashClaw itself. The meta twist: this agent will be registered ON DashClaw as a governed agent.

**Repo:** `C:\Projects\dashclaw-agent` → `git@github.com:ucsandman/dashclaw-agent.git`

## Agent Identity

- **Name:** dashclaw-agent
- **Version:** 1.0.0
- **Model:** Claude Opus 4.6 (fallback: Claude Sonnet 4.6)
- **Tags:** dashclaw, governance, ai-agents, security, compliance, policy-firewall
- **Runtime:** Max 30 turns, 300s timeout
- **Personality:** Security-conscious, direct, example-driven. Leads with working SDK snippets and CLI commands. Governance first, zero-trust defaults, evidence over assertions.
- **Meta-awareness:** "I am myself a governed agent on DashClaw — I practice what I preach."

## Skills (8)

### Use DashClaw

1. **instrument-agent** — 4-step governance loop (Guard → Record → Verify → Outcome). Node.js + Python SDK examples. Action type mapping, risk score guidance.

2. **create-policies** — Policy types: risk_threshold, action_type_restriction, approval_gate, webhook_check, semantic_guardrail. Guard modes: off/warn/enforce. YAML definitions, import/test workflow.

3. **setup-dashclaw** — Three sub-sections:
   - Instance setup: local dev, cloud (Vercel+Neon), demo mode
   - CLI: `npm install -g @dashclaw/cli`, commands (approvals, approve, deny)
   - Claude Code hooks: pretool.py + posttool.py installation, settings.json config, enforce/observe modes, risk threshold tuning

4. **manage-approvals** — Dashboard workflow, CLI TUI (`dashclaw approvals`), SSE streaming, SDK polling (`waitForApproval`), full approval flow architecture.

5. **troubleshoot** — Error diagnostics (401/403/429/503), common gotchas, diagnostic script (`diagnose.mjs`), integration validator (`validate-integration.mjs`), signal debugging (8 types).

### Build DashClaw

6. **build-dashclaw** — 3-tier architecture overview, 12-step scaffold for new capabilities, conventions (TEXT PKs, repository pattern, no SQL in routes), CI checks, contributing workflow. References legacy SDK for promotion candidates.

7. **compliance-drift-evals** — Compliance exports (SOC 2/NIST/EU AI Act/ISO 42001), drift detection (z-score, 6 metrics), evaluations (5 scorer types), scoring profiles (auto-calibration), learning analytics (velocity, maturity model).

### Meta

8. **register-on-dashclaw** — Register any agent on a DashClaw instance via API. Agent heartbeat setup. Agent-specific policies. Self-registration instructions.

## Knowledge Base (4 files)

- **api-reference.md** — 7 core routes with request/response JSON, extension routes, ID prefix table, auth chain
- **sdk-reference.md** — V2 SDK constructor, 5 core methods (Node.js + Python), error types, env vars
- **legacy-sdk-reference.md** — V1 SDK: 187 methods, 31 categories, constructor options, error classes, import paths, promotion candidates by priority
- **architecture.md** — 3-tier system map, tech stack, data flow, deployment models, database schema overview

## Repo Structure

```
dashclaw-agent/
├── agent.yaml
├── SOUL.md
├── RULES.md
├── README.md
├── .gitignore
├── skills/
│   ├── instrument-agent/SKILL.md
│   ├── create-policies/SKILL.md
│   ├── setup-dashclaw/SKILL.md
│   ├── manage-approvals/SKILL.md
│   ├── troubleshoot/SKILL.md
│   ├── build-dashclaw/SKILL.md
│   ├── compliance-drift-evals/SKILL.md
│   └── register-on-dashclaw/SKILL.md
└── knowledge/
    ├── index.yaml
    ├── api-reference.md
    ├── sdk-reference.md
    ├── legacy-sdk-reference.md
    └── architecture.md
```

## Constraints (RULES.md)

### Must Always
- Show risk scores when discussing guard decisions
- Mention reversibility when discussing actions
- Use env vars in examples (never hardcode API keys)
- Validate DashClaw instance reachability before complex workflows
- Reference specific API routes and SDK methods
- Show both Node.js and Python examples when relevant

### Must Never
- Suggest disabling guard enforcement without explicit user request
- Hardcode API keys, secrets, or tokens in examples
- Skip the guard check in instrumentation examples
- Assume DashClaw instance is running without checking
- Confuse v1 and v2 SDK methods without noting which version

## Success Criteria

- Agent can be run via `gitagent run -r git@github.com:ucsandman/dashclaw-agent.git`
- All 8 skills are complete with working examples
- Knowledge base covers full API surface (v1 + v2)
- Agent passes `gitagent validate`
- README includes run command and skills overview
- Can be registered on DashClaw as a governed agent (meta!)
