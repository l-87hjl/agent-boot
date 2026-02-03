# Workspace Initialization

How to set up a new workspace repository from scratch.

---

## Overview

A workspace is any repository where an agent operates. This runbook covers creating the required structure for agent boot compatibility.

**Time Required:** ~10 minutes

**Prerequisites:**
- Git repository (new or existing)
- Access to agent-boot templates
- Knowledge of project name and purpose

---

## Step 1: Create Directory Structure

```bash
# Create agent memory directory
mkdir -p agent

# Create workspace directories
mkdir -p inputs outputs audits

# Create scratch directory (will be gitignored)
mkdir -p scratch
```

**Result:**
```
your-repo/
├── agent/          # Agent memory files
├── inputs/         # Input data
├── outputs/        # Generated artifacts
├── audits/         # Audit logs
└── scratch/        # Temporary files
```

---

## Step 2: Update .gitignore

```bash
cat >> .gitignore << 'EOF'

# Agent scratch space
scratch/

# OS files
.DS_Store
Thumbs.db

# Editor files
*.swp
*~
EOF
```

---

## Step 3: Create AGENT_LINK.md

Copy the template and customize:

```bash
# Fetch template
curl -o AGENT_LINK.md \
  "https://raw.githubusercontent.com/l-87hjl/agent-boot/main/main/agent-boot/templates/AGENT_LINK.template.md"
```

Edit AGENT_LINK.md and replace placeholders:

| Placeholder | Replace With |
|-------------|--------------|
| `{{BOOT_OWNER}}` | `l-87hjl` |
| `{{BOOT_REPO}}` | `agent-boot` |
| `{{BOOT_REF}}` | `main` |
| `{{BOOT_VERSION}}` | `1.0.0` |
| `{{CONTRACT_REPO}}` | `l-87hjl/ai-agent-contract` |

**Example Result:**
```markdown
| Boot Repository | `l-87hjl/agent-boot` |
| Boot Reference | `main` |
| Boot Version | `1.0.0` |
```

---

## Step 4: Initialize STATE.json

```bash
# Fetch template
curl -o agent/STATE.json \
  "https://raw.githubusercontent.com/l-87hjl/agent-boot/main/main/agent-boot/templates/STATE.template.json"
```

Edit agent/STATE.json:

```json
{
  "version": "1.0.0",
  "lastUpdated": "2026-02-01T00:00:00Z",
  "lastCommitSha": null,
  "activeProject": "your-project-name",
  "currentPhase": "idle",
  "nextIntent": "",
  "errors": [],
  "metadata": {}
}
```

**Important:** Remove `_comment` and `_instructions` fields.

---

## Step 5: Initialize TODO.json

```bash
# Fetch template
curl -o agent/TODO.json \
  "https://raw.githubusercontent.com/l-87hjl/agent-boot/main/main/agent-boot/templates/TODO.template.json"
```

Edit agent/TODO.json:

```json
{
  "version": "1.0.0",
  "tasks": [
    {
      "id": "init-001",
      "title": "Complete workspace setup",
      "status": "in-progress",
      "priority": "high",
      "description": "Finish initializing agent workspace",
      "dependencies": [],
      "createdAt": "2026-02-01T00:00:00Z",
      "updatedAt": "2026-02-01T00:00:00Z",
      "completedAt": null
    }
  ],
  "metadata": {
    "projectName": "your-project-name"
  }
}
```

**Important:** Remove `_comment` and `_instructions` fields.

---

## Step 6: Initialize CHANGELOG.md

```bash
# Fetch template
curl -o CHANGELOG.md \
  "https://raw.githubusercontent.com/l-87hjl/agent-boot/main/main/agent-boot/templates/CHANGELOG.template.md"
```

Edit CHANGELOG.md:

```markdown
# Changelog

All notable changes to this project will be documented in this file.

---

## 2026-02-01

- Initialized agent workspace
- Created agent memory files (STATE.json, TODO.json)
- Linked to agent-boot contract v1.0.0

---
```

---

## Step 7: Validate Setup

### Check structure:
```bash
ls -la
# Should show: AGENT_LINK.md, CHANGELOG.md, agent/, inputs/, outputs/, audits/

ls -la agent/
# Should show: STATE.json, TODO.json
```

### Validate JSON files:
```bash
# Parse test
cat agent/STATE.json | jq .
cat agent/TODO.json | jq .
```

### Schema validation (if ajv installed):
```bash
ajv validate -s schemas/state-schema.json -d agent/STATE.json
ajv validate -s schemas/todo-schema.json -d agent/TODO.json
```

---

## Step 8: Initial Commit

```bash
# Stage files
git add AGENT_LINK.md CHANGELOG.md agent/ .gitignore

# Commit
git commit -m "Initialize agent workspace

- Add AGENT_LINK.md referencing agent-boot v1.0.0
- Create agent memory files (STATE.json, TODO.json)
- Set up workspace directories
- Initialize CHANGELOG.md"

# Push
git push
```

---

## Step 9: Verify Boot Connection

Run the startup ritual to confirm everything works:

```bash
# Refer to runbooks/startup-ritual.md
# Should complete all 7 steps successfully
```

---

## Final Structure

```
your-repo/
├── AGENT_LINK.md           # Boot configuration
├── CHANGELOG.md            # Change history
├── .gitignore              # Includes scratch/
├── agent/
│   ├── STATE.json          # Runtime state
│   └── TODO.json           # Task tracking
├── inputs/                 # Input data
├── outputs/                # Generated artifacts
├── audits/                 # Audit logs
└── scratch/                # Temp files (gitignored)
```

---

## Checklist

- [ ] Directory structure created
- [ ] .gitignore updated
- [ ] AGENT_LINK.md created with correct values
- [ ] agent/STATE.json initialized and valid
- [ ] agent/TODO.json initialized and valid
- [ ] CHANGELOG.md created with initial entry
- [ ] All files committed and pushed
- [ ] Startup ritual completes successfully

---

## Troubleshooting

### "Cannot fetch templates"
- Check network connectivity
- Verify agent-boot repo is public
- Use raw.githubusercontent.com URLs

### "JSON parse error"
- Ensure all placeholders replaced
- Remove comment fields from templates
- Check for trailing commas

### "Schema validation fails"
- Verify version field matches schema version
- Check required fields are present
- Ensure enum values are exact matches

### "Startup ritual fails"
- Review each step's expected output
- Check AGENT_LINK.md formatting
- Ensure boot version matches

---

## Next Steps

After initialization:

1. Review `contract/AGENT_RULES.md` for behavioral expectations
2. Review `contract/LOOP_PROTOCOL.md` for execution cycle
3. Add project-specific tasks to TODO.json
4. Begin work following the loop protocol
