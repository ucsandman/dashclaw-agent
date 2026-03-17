---
name: instrument-agent
description: Integrate DashClaw SDK into any agent using the 4-step governance loop
category: integration
---

# Instrument Your Agent with DashClaw

Help developers add DashClaw governance to any AI agent. Walk through the 4-step governance loop with working code.

## The 4-Step Governance Loop

Every governed decision follows this deterministic flow:

```
1. Guard  → "Can I do this?"           (POST /api/guard)
2. Record → "I am doing this."         (POST /api/actions)
3. Verify → "I believe this is true."  (POST /api/assumptions)
4. Outcome → "This was the result."    (PATCH /api/actions/:id)
```

## Step 0: Install & Initialize

### Node.js
```bash
npm install dashclaw
```

```javascript
import { DashClaw } from 'dashclaw';

const claw = new DashClaw({
  baseUrl: process.env.DASHCLAW_BASE_URL,
  apiKey: process.env.DASHCLAW_API_KEY,
  agentId: 'my-agent'
});
```

### Python
```bash
pip install dashclaw
```

```python
from dashclaw import DashClaw

claw = DashClaw(
    base_url=os.environ["DASHCLAW_BASE_URL"],
    api_key=os.environ["DASHCLAW_API_KEY"],
    agent_id="my-agent"
)
```

## Step 1: Guard — Check Policy Before Acting

```javascript
const decision = await claw.guard({
  action_type: 'deploy',
  declared_goal: 'Deploy build #402 to production',
  risk_score: 85,
  systems_touched: ['production', 'database'],
  reversible: false
});

// decision.decision: 'allow' | 'warn' | 'block' | 'require_approval'
if (decision.decision === 'block') {
  console.log('Blocked:', decision.reason);
  return;
}
```

```python
decision = claw.guard(
    action_type="deploy",
    declared_goal="Deploy build #402 to production",
    risk_score=85,
    systems_touched=["production", "database"],
    reversible=False
)

if decision["decision"] == "block":
    print(f"Blocked: {decision['reason']}")
    return
```

**Guard response shape:**
```json
{
  "decision": "require_approval",
  "action_id": "act_gd_abc123",
  "reason": "Risk score exceeds org threshold",
  "signals": ["Production access", "High risk score"],
  "risk_score": 75,
  "agent_risk_score": 85
}
```

## Step 2: Record — Log the Action

```javascript
const action = await claw.createAction({
  action_type: 'deploy',
  declared_goal: 'Deploy build #402 to production',
  risk_score: 85,
  reversible: false,
  systems_touched: ['production']
});
// action.action_id: 'ar_abc123'
```

```python
action = claw.create_action(
    action_type="deploy",
    declared_goal="Deploy build #402 to production",
    risk_score=85,
    reversible=False,
    systems_touched=["production"]
)
```

## Step 3: Verify — Record Assumptions

```javascript
await claw.recordAssumption({
  action_id: action.action_id,
  assumption: 'Staging tests passed successfully',
  source: 'ci-pipeline'
});
```

```python
claw.record_assumption(
    action_id=action["action_id"],
    assumption="Staging tests passed successfully",
    source="ci-pipeline"
)
```

## Step 4: Outcome — Record the Result

```javascript
await claw.updateOutcome(action.action_id, {
  status: 'completed',        // or 'failed'
  output_summary: 'Build #402 deployed successfully to production',
  timestamp_end: new Date().toISOString()
});
```

```python
claw.update_outcome(action["action_id"], {
    "status": "completed",
    "output_summary": "Build #402 deployed successfully to production"
})
```

## Complete Example

```javascript
import { DashClaw } from 'dashclaw';

const claw = new DashClaw({
  baseUrl: process.env.DASHCLAW_BASE_URL,
  apiKey: process.env.DASHCLAW_API_KEY,
  agentId: 'deploy-agent'
});

async function governedDeploy(buildId) {
  // 1. Guard
  const decision = await claw.guard({
    action_type: 'deploy',
    declared_goal: `Deploy build #${buildId} to production`,
    risk_score: 85,
    systems_touched: ['production'],
    reversible: false
  });

  if (decision.decision === 'block') {
    console.log('Blocked:', decision.reason);
    return;
  }

  // 2. Record
  const action = await claw.createAction({
    action_type: 'deploy',
    declared_goal: `Deploy build #${buildId} to production`,
    risk_score: 85,
    reversible: false
  });

  // 3. Verify assumptions
  await claw.recordAssumption({
    action_id: action.action_id,
    assumption: 'All CI checks passed'
  });

  // 4. Execute and record outcome
  try {
    await actualDeploy(buildId);
    await claw.updateOutcome(action.action_id, {
      status: 'completed',
      output_summary: `Build #${buildId} deployed successfully`
    });
  } catch (err) {
    await claw.updateOutcome(action.action_id, {
      status: 'failed',
      output_summary: err.message
    });
  }
}
```

## Action Type & Risk Score Guide

| Action Type | Risk Score | Reversible | Example |
|------------|-----------|------------|---------|
| deploy | 75-90 | false | Production deployment |
| api_call | 20-40 | true | External API request |
| file_write | 15-30 | true | Local file modification |
| database | 50-80 | false | Schema migration, data deletion |
| security | 80-95 | false | Key rotation, permission changes |
| build | 10-25 | true | npm install, compilation |
| notify | 5-15 | true | Send email, Slack message |

**Risk scoring rule:** DashClaw uses the HIGHER of computed risk and agent-reported risk. Always report honestly — inflating risk is better than under-reporting.

## HITL Approval Flow

When guard returns `require_approval`:

```javascript
if (decision.decision === 'require_approval') {
  console.log('Waiting for human approval...');
  await claw.waitForApproval(decision.action_id, {
    timeout: 300000  // 5 minutes
  });
  // Continues after approval, throws ApprovalDeniedError if denied
}
```

## Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `DASHCLAW_BASE_URL` | Yes | DashClaw instance URL |
| `DASHCLAW_API_KEY` | Yes | API authentication key |
| `DASHCLAW_AGENT_ID` | No | Default agent identifier |

## Validation

After instrumenting, run the integration validator:
```bash
node scripts/validate-integration.mjs --base-url $DASHCLAW_BASE_URL --api-key $DASHCLAW_API_KEY
```
