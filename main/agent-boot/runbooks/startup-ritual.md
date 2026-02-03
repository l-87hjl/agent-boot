# Startup Ritual

Step-by-step protocol for agent initialization at the start of every session.

---

## Prerequisites

- Git access to workspace repository
- Git access to agent-boot repository (read-only)
- Network connectivity for fetching remote files

---

## Step 1: Verify Workspace

**Goal:** Confirm we're in a valid workspace with agent configuration.

```bash
# Check for AGENT_LINK.md
ls -la AGENT_LINK.md
```

**Expected Output:**
```
-rw-r--r-- 1 user user 1234 Jan 15 10:00 AGENT_LINK.md
```

**If Missing:**
```
ABORT: No AGENT_LINK.md found.
This repository is not configured for agent operation.
See runbooks/workspace-initialization.md to set up.
```

---

## Step 2: Parse Agent Link

**Goal:** Extract boot configuration from AGENT_LINK.md.

```bash
# Read boot configuration
cat AGENT_LINK.md | grep -E "Boot (Repository|Reference|Version)"
```

**Expected Output:**
```
| Boot Repository | `l-87hjl/agent-boot` |
| Boot Reference | `main` |
| Boot Version | `1.0.0` |
```

**Extract Values:**
- `BOOT_OWNER` = l-87hjl
- `BOOT_REPO` = agent-boot
- `BOOT_REF` = main
- `BOOT_VERSION` = 1.0.0

**If Parse Fails:**
```
ABORT: Could not parse AGENT_LINK.md.
File may be corrupted or using incompatible format.
```

---

## Step 3: Verify Boot Repository

**Goal:** Confirm boot repo is accessible and version matches.

```bash
# Fetch VERSION.json from boot repo
curl -s "https://raw.githubusercontent.com/${BOOT_OWNER}/${BOOT_REPO}/${BOOT_REF}/main/agent-boot/VERSION.json"
```

**Expected Output:**
```json
{
  "bootVersion": "1.0.0",
  "lastUpdated": "2026-02-01",
  "compatibleWorkspaces": ["1.0.x"]
}
```

**Validation:**
1. `bootVersion` must match `BOOT_VERSION` from AGENT_LINK.md
2. Workspace version must be in `compatibleWorkspaces` range

**If Version Mismatch:**
```
ABORT: Boot version mismatch.
Expected: 1.0.0
Found: 2.0.0
Action: Update AGENT_LINK.md or use compatible boot ref.
```

---

## Step 4: Load State

**Goal:** Read and validate STATE.json.

```bash
# Check if state file exists
ls -la agent/STATE.json
```

**If Exists:**
```bash
# Read and validate
cat agent/STATE.json | jq .
```

**Expected Output:**
```json
{
  "version": "1.0.0",
  "lastUpdated": "2026-01-15T10:00:00Z",
  "lastCommitSha": "abc1234",
  "activeProject": "my-project",
  "currentPhase": "implementing",
  "nextIntent": "Continue feature implementation",
  "errors": [],
  "metadata": {}
}
```

**If Missing (First Run):**
```bash
# Initialize from template
mkdir -p agent
cp templates/STATE.template.json agent/STATE.json
# Edit to replace placeholders
```

**If Invalid JSON:**
```
See runbooks/error-recovery.md for STATE.json recovery.
```

---

## Step 5: Load TODO

**Goal:** Read and validate TODO.json.

```bash
cat agent/TODO.json | jq .
```

**Expected Output:**
```json
{
  "version": "1.0.0",
  "tasks": [
    {
      "id": "task-001",
      "title": "Implement feature X",
      "status": "in-progress",
      "priority": "high"
    }
  ]
}
```

**If Missing:**
```bash
cp templates/TODO.template.json agent/TODO.json
```

---

## Step 6: Validate Against Schemas

**Goal:** Ensure state files conform to boot schemas.

```bash
# Validate STATE.json (using ajv or similar)
ajv validate -s schemas/state-schema.json -d agent/STATE.json

# Validate TODO.json
ajv validate -s schemas/todo-schema.json -d agent/TODO.json
```

**Expected Output:**
```
agent/STATE.json valid
agent/TODO.json valid
```

**If Validation Fails:**
```
See runbooks/error-recovery.md for recovery procedures.
```

---

## Step 7: Report Ready

**Goal:** Confirm successful initialization.

```
✓ Workspace verified: AGENT_LINK.md present
✓ Boot loaded: l-87hjl/agent-boot@main v1.0.0
✓ State loaded: currentPhase=implementing
✓ TODO loaded: 3 pending, 1 in-progress, 5 done
✓ Schemas validated

Ready to proceed.
Previous intent: "Continue feature implementation"
```

---

## Quick Reference

| Step | Check | Failure Action |
|------|-------|----------------|
| 1 | AGENT_LINK.md exists | ABORT - not a workspace |
| 2 | Parse boot config | ABORT - corrupted config |
| 3 | Version match | ABORT - upgrade needed |
| 4 | STATE.json valid | Initialize or recover |
| 5 | TODO.json valid | Initialize or recover |
| 6 | Schema validation | See error-recovery.md |
| 7 | Report status | Proceed to work |

---

## Timing

This ritual should complete in < 30 seconds under normal conditions.
If any step hangs, check network connectivity.
