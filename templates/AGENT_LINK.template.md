# Agent Link

This file defines the boot contract for AI agents working in this repository.

## Boot Configuration

| Field | Value |
|-------|-------|
| Boot Repository | `{{BOOT_OWNER}}/{{BOOT_REPO}}` |
| Boot Reference | `{{BOOT_REF}}` |
| Boot Version | `{{BOOT_VERSION}}` |
| Contract Repository | `{{CONTRACT_REPO}}` |

## Memory Files

The agent persists state in these locations:

| Purpose | Path |
|---------|------|
| State | `agent/STATE.json` |
| Tasks | `agent/TODO.json` |
| Changelog | `CHANGELOG.md` |

## Workspace Directories

| Directory | Purpose |
|-----------|---------|
| `agent/` | Agent memory and state files |
| `inputs/` | Input data for processing |
| `outputs/` | Generated outputs and artifacts |
| `audits/` | Audit logs and compliance records |
| `scratch/` | Temporary working files (gitignored) |

## Validation

Before starting work, the agent must verify:

1. [ ] Boot repository is accessible
2. [ ] Boot version matches expectation (`{{BOOT_VERSION}}`)
3. [ ] All memory files exist (or can be initialized)
4. [ ] State validates against `schemas/state-schema.json`
5. [ ] TODO validates against `schemas/todo-schema.json`

## Instructions

To use this template:

1. Copy to your workspace root as `AGENT_LINK.md`
2. Replace all `{{PLACEHOLDER}}` values:
   - `{{BOOT_OWNER}}` → `l-87hjl`
   - `{{BOOT_REPO}}` → `agent-boot`
   - `{{BOOT_REF}}` → `main`
   - `{{BOOT_VERSION}}` → `1.0.0`
   - `{{CONTRACT_REPO}}` → `l-87hjl/ai-agent-contract`
3. Create the required directories
4. Initialize `agent/STATE.json` from template
5. Initialize `agent/TODO.json` from template
