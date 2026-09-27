<div align="center">

<img src="https://static.wixstatic.com/media/aec640_ec489547dc9847f4aceef2969b8376e2~mv2.png" alt="Infuse Logo" width="220" />

# Infuse for Claude & Claude Code

### Execution Intelligence for AI Agent Workflows

[![Version](https://img.shields.io/badge/version-0.1.0-blue.svg)](https://github.com/adorbistech/infuse-claude)
[![License](https://img.shields.io/badge/license-Apache--2.0-green.svg)](LICENSE)
[![MCP Endpoint](https://img.shields.io/badge/MCP-Production-brightgreen.svg)](https://infuse-api.adorbistech.com/mcp)

</div>

---

## Overview

**Infuse** provides real-time execution intelligence, deterministic state tracking, event telemetry, and policy-governed control for AI agent workflows.

This repository contains the official **Infuse adapter plugin for Claude and Claude Code**, connecting Anthropic's Claude ecosystem directly to the production Infuse governance and execution engine over the Model Context Protocol (MCP).

---

## Architecture

Following the **One Core, Many Adapters** architecture, this repository acts purely as a protocol adapter. All execution states, policy evaluations, and Governor decisions remain strictly authoritative within the Infuse core engine.

```text
Claude / Claude Code
        │
        │ MCP (Stream / JSON-RPC)
        ▼
   Infuse MCP (https://infuse-api.adorbistech.com/mcp)
        │
        ▼
   Infuse Core (State Machine, Policy Engine, Governor)
```

---

## Key Capabilities

- **Execution State Inspection**: Query real-time workflow status, lifecycle transitions, and execution histories.
- **Result Verification**: Retrieve definitive execution outputs without model hallucination or assumption.
- **Event Telemetry**: Stream and publish domain events across agent workflow steps.
- **Policy & Governance Enforcement**: Inspect active security and operational constraints before actions are taken.
- **Governor Verdicts**: Retrieve autonomous Governor verdicts and rationale.
- **Controlled Interventions**: Execute explicit, guarded control actions (pause, resume, abort, cancel) under strict human-in-the-loop boundaries.

---

## Production Endpoint

The plugin connects directly to the production Infuse MCP server:

```text
https://infuse-api.adorbistech.com/mcp
```

---

## Plugin Structure

```text
infuse-claude/
├── .claude-plugin/
│   └── plugin.json        # Claude plugin manifest
├── .mcp.json              # MCP connection definition
├── skills/
│   └── infuse/
│       └── SKILL.md       # Concise guidance and tool routing
├── evals/
│   └── evals.json         # Behavioral evaluation suite
├── README.md              # Documentation
└── LICENSE                # Apache-2.0 License
```

---

## Tool Reference & Semantics

The Infuse MCP server provides a standardized tool suite categorized by operational boundary:

### 1. Read-Only Inspection Tools
Safe, non-mutating queries for execution status, logs, policies, and Governor verdicts:
- `infuse_get_execution` — Retrieve complete details of an execution record.
- `infuse_get_execution_state` — Query the current lifecycle phase of an execution.
- `infuse_get_execution_result` — Query the final output and result payload.
- `infuse_list_executions` — Search and filter execution history.
- `infuse_list_events` — Query telemetry and lifecycle event logs.
- `infuse_get_policy` — Inspect specific policy definitions and constraints.
- `infuse_list_policies` — List all registered governance policies.
- `infuse_get_governor_decision` — Fetch Governor decisions and justification.

### 2. Mutating Tools (Non-Destructive)
Tools that initiate workflows or record telemetry without terminating operations:
- `infuse_execute` — Launch a governed execution workflow.
- `infuse_publish_event` — Emit an audit or telemetry event into the stream.
- `infuse_update_policy` — Update a policy configuration.

### 3. Destructive Control Boundary
- `infuse_control` — **Critical control operations** (abort, pause, resume, cancel, terminate).
  > **Safety Rule**: `infuse_control` is invoked only when explicitly instructed by the user to perform an operational intervention on a running or stalled workflow.

---

## Quick Start

### Installation in Claude Code

1. Clone or add the plugin directory to your Claude Code workspace:
   ```bash
   claude --plugin-dir /path/to/infuse-claude
   ```

2. Verify plugin and MCP connectivity inside Claude Code:
   ```text
   /plugin
   /mcp
   ```

3. Confirm that `infuse` tools are available and active.

---

## Behavioral Evaluations

The evaluation suite located in [`evals/evals.json`](file:///Volumes/Adorbis/infuse-claude/evals/evals.json) tests 10 core integration requirements:
1. **Execution inspection** — Queries state via Infuse tools rather than inventing answers.
2. **Execution state** — Retrieves authoritative lifecycle states.
3. **Execution result** — Fetches true output payloads without fabrication.
4. **Policy inspection** — Verifies constraints against registered Infuse policies.
5. **Governor decision** — Queries the Governor engine for verdict rationale.
6. **Read-only boundary** — Confines read queries to non-mutating tools.
7. **Mutating operations** — Correctly routes execution commands.
8. **Destructive control** — Restricts `infuse_control` to explicit intervention requests.
9. **No fabricated success** — Never claims completion without confirmed tool results.
10. **Governance preservation** — Adheres strictly to Infuse policy constraints.

---

## Links & Ecosystem

- **Infuse Platform**: [https://infuse.adorbistech.com](https://infuse.adorbistech.com)
- **Infuse Core Repository**: [https://github.com/adorbistech/infuse](https://github.com/adorbistech/infuse)
- **Claude Plugin Repository**: [https://github.com/adorbistech/infuse-claude](https://github.com/adorbistech/infuse-claude)
- **Adorbis Tech**: [https://adorbistech.com](https://adorbistech.com)

---

## License

This project is licensed under the Apache-2.0 License. See the [LICENSE](file:///Volumes/Adorbis/infuse-claude/LICENSE) file for details.
