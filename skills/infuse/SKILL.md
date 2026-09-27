---
name: infuse
description: Execution intelligence and governance for AI agent workflows. Authoritative guide for routing execution inspection, policy retrieval, Governor decisions, and execution control through Infuse MCP.
---

# Infuse Skill

## Source of Truth

The Infuse MCP server (`https://infuse-api.adorbistech.com/mcp`) is the authoritative source of truth for:
- Execution state & status
- Execution results & outputs
- Execution history & timelines
- Event publishing & telemetry
- Workflow policies & constraints
- Governor evaluation decisions & enforcement

Never fabricate, predict, or invent execution states, results, or Governor decisions. Always retrieve the actual record via Infuse tools.

## Core Behavioral Guidelines

1. **Query Authoritative State**: When asked what happened in a workflow or execution, call the relevant Infuse inspection tool before responding.
2. **Track Execution Identifiers**: Always capture and propagate `execution_id` values returned from `infuse_execute` or `infuse_list_executions` to subsequent tool calls.
3. **Inspect Before Assuming**: Retrieve policies with `infuse_get_policy` and Governor verdicts with `infuse_get_governor_decision` rather than assuming policy rules or governance outcomes.
4. **No Premature Success Claims**: Never claim an execution or action succeeded without receiving a confirmed success status in the tool response.
5. **Preserve Governance Boundaries**: Do not bypass, simulate, or circumvent Infuse governance policies or Governor checks.

## Tool Categories and Boundaries

### Read-Only Tools (Safe for Inspection)
Use these when retrieving status, logs, decisions, or metadata:
- `infuse_get_execution` — Fetch full details of a specific execution.
- `infuse_get_execution_state` — Query current lifecycle state of an execution.
- `infuse_get_execution_result` — Query terminal result or output of an execution.
- `infuse_list_executions` — List executions matching filter criteria.
- `infuse_list_events` — Fetch event telemetry associated with executions.
- `infuse_get_policy` — Inspect an active policy definition.
- `infuse_list_policies` — List all registered governance policies.
- `infuse_get_governor_decision` — Fetch Governor decision and rationale for an execution.

### Mutating Tools (Non-Destructive State Transitions)
Use when initiating workflows or recording events:
- `infuse_execute` — Launch an execution managed by Infuse.
- `infuse_publish_event` — Emit a lifecycle or audit event into the Infuse event stream.
- `infuse_update_policy` — Modify a governance policy configuration.

### Destructive Control Boundary
- `infuse_control` — **Destructive control operation.**
  - Performs critical interventions: pause, resume, abort, cancel, or terminate executions.
  - **Constraint**: Only invoke `infuse_control` when the user explicitly requests an operational intervention on a running or stalled execution.
