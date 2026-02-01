# Agent Boot

A centralized repository for AI agent configuration, rules, and templates. Projects reference this repo to bootstrap agent behavior consistently across multiple repositories.

## Structure

```
agent-boot/
├── profiles/           # Per-project agent configurations
│   ├── default/        # Default/fallback profile with templates
│   ├── story-grader/   # l-87hjl/story-grader
│   ├── rule-based-horror/  # l-87hjl/rule-based-horror
│   ├── Medium/         # l-87hjl/Medium
│   ├── 3i-atlas-public-data/  # l-87hjl/3i-atlas-public-data
│   ├── PNP/            # l-87hjl/PNP
│   ├── Covenant/       # l-87hjl/Covenant
│   ├── ai-emergence-under-constraint/  # l-87hjl/ai-emergence-under-constraint
│   ├── novel-completer/    # l-87hjl/novel-completer
│   └── ai_emergence_simulator/  # l-87hjl/ai_emergence_simulator
└── shared/             # Common conventions across all projects
    ├── commit_message_style.md
    └── file_naming.md
```

## Quick Start

### 1. Add AGENT_CONFIG.json to Your Project

Copy the appropriate `AGENT_CONFIG.json` from your profile folder to your project's root:

```bash
# Example for story-grader project
curl -o AGENT_CONFIG.json \
  https://raw.githubusercontent.com/l-87hjl/agent-boot/main/main/agent-boot/profiles/story-grader/AGENT_CONFIG.json
```

Or manually create `AGENT_CONFIG.json` in your project root:

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
      "state": "docs/memory/STATE.md",
      "todo": "docs/memory/TODO.md",
      "changelog": "CHANGELOG.md"
    }
  }
}
```

### 2. Create Memory Files (Optional)

If using memory tracking, create the memory files in your project:

```bash
mkdir -p docs/memory
touch docs/memory/STATE.md
touch docs/memory/TODO.md
touch CHANGELOG.md
```

## Configuration Reference

### AGENT_CONFIG.json Schema

| Field | Description |
|-------|-------------|
| `boot.owner` | GitHub owner of the agent-boot repo |
| `boot.repo` | Repository name (agent-boot) |
| `boot.ref` | Branch/tag to use (typically "main") |
| `boot.profile` | Profile name matching your project |
| `project.defaultBranch` | Your project's default branch |
| `project.memory.state` | Path to STATE.md for runtime state |
| `project.memory.todo` | Path to TODO.md for task tracking |
| `project.memory.changelog` | Path to CHANGELOG.md for history |

## Profiles

### default
The fallback profile containing:
- `AGENT_RULES.md` - Core behavioral rules
- `LOOP.md` - OODA-style execution loop
- `STOP_LINES.md` - Termination conditions
- `TEMPLATES/` - Templates for STATE, TODO, CHANGELOG, etc.

### story-grader
For the story grading project:
- `RUNBOOK.md` - Grading procedures
- `RUBRIC_SCHEMA.md` - Evaluation rubric format

### rule-based-horror
For the horror writing audit project:
- `RUNBOOK.md` - Audit procedures
- `AUDIT_CHECKLIST.md` - Quality checklist

### Other Profiles
Each project-specific profile contains:
- `AGENT_CONFIG.json` - Pre-configured for that repo
- `RUNBOOK.md` - Project-specific procedures

## Shared Conventions

All profiles inherit from `shared/`:
- **commit_message_style.md** - Git commit conventions
- **file_naming.md** - File and directory naming standards

## Excluded Projects

The following repos do not have profiles (infrastructure/meta):
- `repo-bridge` - Repository bridging infrastructure
- `agent-boot` - This repository (self-reference)
- `ai-agent-contract` - Agent contract definitions

## Adding a New Profile

1. Create a directory under `profiles/` matching your repo name
2. Add `AGENT_CONFIG.json` configured for the repo
3. Add `RUNBOOK.md` with project-specific procedures
4. Add any additional profile-specific files
5. Update this README

## Usage Pattern

```
┌─────────────────┐     references      ┌─────────────────┐
│  Your Project   │ ──────────────────► │   agent-boot    │
│                 │                     │                 │
│ AGENT_CONFIG.json                     │ profiles/       │
│                 │     loads           │   your-project/ │
│                 │ ◄────────────────── │     RUNBOOK.md  │
│                 │                     │     ...         │
└─────────────────┘                     └─────────────────┘
```

The agent reads `AGENT_CONFIG.json` from your project, fetches the corresponding profile from agent-boot, and applies those rules/templates during execution.
