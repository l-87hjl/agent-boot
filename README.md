# Agent Boot

**Version:** 1.0.0

A read-only contract repository containing rules, protocols, schemas, and templates that govern AI agent behavior across all workspaces.

> **Read-Only Enforcement:** This repository is enforced read-only via `repo-bridge` (`READ_ONLY_REPOS=l-87hjl/agent-boot`). Agents must never write to this repository. All state, todos, and changelogs live in workspace repositories.

**Related:** See [ai-agent-contract](https://github.com/l-87hjl/ai-agent-contract) for formal guarantees and contract definitions.

---

## Structure

```
agent-boot/
├── VERSION.json              # Boot version and compatibility
├── AGENT_ENTRY.md            # Agent's starting point
├── contract/                 # Mandatory agent rules
│   ├── AGENT_RULES.md        # Core behavioral rules
│   ├── LOOP_PROTOCOL.md      # Read→Plan→Act→Verify→Persist cycle
│   └── SAFETY_CONSTRAINTS.md # What agent must never do
├── schemas/                  # JSON Schema definitions
│   ├── state-schema.json     # STATE.json validation
│   └── todo-schema.json      # TODO.json validation
├── templates/                # Files to copy into workspaces
│   ├── AGENT_LINK.template.md
│   ├── STATE.template.json
│   ├── TODO.template.json
│   └── CHANGELOG.template.md
├── runbooks/                 # Operational procedures
│   ├── startup-ritual.md     # Session initialization
│   ├── error-recovery.md     # Recovery procedures
│   └── workspace-initialization.md
├── profiles/                 # Per-project configurations
│   ├── default/              # Fallback profile with templates
│   ├── story-grader/
│   ├── rule-based-horror/
│   ├── Medium/
│   ├── 3i-atlas-public-data/
│   ├── PNP/
│   ├── Covenant/
│   ├── ai-emergence-under-constraint/
│   ├── novel-completer/
│   └── ai_emergence_simulator/
└── shared/                   # Common conventions
    ├── commit_message_style.md
    └── file_naming.md
```

---

## Quick Start

### For New Workspaces

Follow `runbooks/workspace-initialization.md` to set up a new agent workspace.

**Summary:**
1. Create `AGENT_LINK.md` from template (points to this repo)
2. Create `agent/STATE.json` from template
3. Create `agent/TODO.json` from template
4. Create `CHANGELOG.md` from template
5. Commit and push

### For Existing Projects

Add `AGENT_CONFIG.json` to your project root:

```bash
# Example for story-grader project
curl -o AGENT_CONFIG.json \
  "https://raw.githubusercontent.com/l-87hjl/agent-boot/main/profiles/story-grader/AGENT_CONFIG.json"
```

---

## Core Concepts

### The Loop Protocol

Every agent session follows a 5-step cycle:

```
1. READ     → Load state, understand context
2. PLAN     → Decide what to do
3. ACT      → Execute the plan
4. VERIFY   → Confirm success
5. PERSIST  → Save state, commit, push
```

See `contract/LOOP_PROTOCOL.md` for details.

### The Core Rules

1. **Entry Point:** Always read `AGENT_ENTRY.md` first
2. **Read-Only Boot:** Never write to agent-boot
3. **Schema Validation:** Validate state files before action
4. **Changelog Discipline:** Update changelog on every commit
5. **Fail Fast:** Abort if context is incomplete
6. **Version Compatibility:** Boot version must match expectations

See `contract/AGENT_RULES.md` for full details.

### Safety Constraints

Absolute prohibitions the agent must never violate. See `contract/SAFETY_CONSTRAINTS.md`.

---

## Configuration Reference

### VERSION.json

```json
{
  "bootVersion": "1.0.0",
  "lastUpdated": "2026-02-01",
  "compatibleWorkspaces": ["1.0.x"]
}
```

### AGENT_CONFIG.json (in workspace)

```json
{
  "boot": {
    "owner": "l-87hjl",
    "repo": "agent-boot",
    "ref": "main",
    "profile": "your-profile-name"
  },
  "project": {
    "defaultBranch": "main",
    "memory": {
      "state": "agent/STATE.json",
      "todo": "agent/TODO.json",
      "changelog": "CHANGELOG.md"
    }
  }
}
```

---

## Profiles

### default
The fallback profile containing:
- `AGENT_RULES.md` - Core behavioral rules
- `LOOP.md` - OODA-style execution loop
- `STOP_LINES.md` - Termination conditions
- `TEMPLATES/` - Templates for STATE, TODO, CHANGELOG, etc.

### story-grader
- `RUNBOOK.md` - Grading procedures
- `RUBRIC_SCHEMA.md` - Evaluation rubric format

### rule-based-horror
- `RUNBOOK.md` - Audit procedures
- `AUDIT_CHECKLIST.md` - Quality checklist

### Other Profiles
Each project-specific profile contains:
- `AGENT_CONFIG.json` - Pre-configured for that repo
- `RUNBOOK.md` - Project-specific procedures

---

## Schemas

All state files must validate against JSON Schema Draft 7:

| File | Schema |
|------|--------|
| `agent/STATE.json` | `schemas/state-schema.json` |
| `agent/TODO.json` | `schemas/todo-schema.json` |

Validation is mandatory before any agent action.

---

## Runbooks

| Runbook | Purpose |
|---------|---------|
| `startup-ritual.md` | Step-by-step session initialization |
| `error-recovery.md` | Recovery from common errors |
| `workspace-initialization.md` | Setting up new workspaces |

---

## Excluded Projects

The following repos do not have profiles (infrastructure/meta):
- `repo-bridge` - Repository bridging infrastructure
- `agent-boot` - This repository (self-reference)
- `ai-agent-contract` - Agent contract definitions

---

## Adding a New Profile

1. Create a directory under `profiles/` matching your repo name
2. Add `AGENT_CONFIG.json` configured for the repo
3. Add `RUNBOOK.md` with project-specific procedures
4. Add any additional profile-specific files
5. Update this README

---

## Usage Pattern

```
┌─────────────────┐                      ┌─────────────────┐
│   Workspace     │      references      │   agent-boot    │
│                 │ ────────────────────►│   (read-only)   │
│ AGENT_LINK.md   │                      │                 │
│ agent/STATE.json│      validates       │ schemas/        │
│ agent/TODO.json │ ◄────────────────────│ contract/       │
│ CHANGELOG.md    │                      │ templates/      │
└─────────────────┘                      └─────────────────┘
```

The agent:
1. Reads `AGENT_ENTRY.md` from boot repo
2. Reads `AGENT_LINK.md` from workspace
3. Validates workspace state against schemas
4. Applies rules and protocols during execution
5. Persists state back to workspace (never to boot)

---

## For Humans

If you need to update agent rules or protocols:
1. Make changes in this repository
2. Test with the agent to ensure behavior is correct
3. Document changes in commit messages

The agent will pick up new rules on its next session start.
