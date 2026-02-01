# Error Recovery

Procedures for recovering from common error conditions.

---

## Error Index

| Error | Severity | Recovery |
|-------|----------|----------|
| Missing AGENT_LINK.md | Critical | [Cannot proceed](#missing-agent_linkmd) |
| Corrupted STATE.json | High | [Restore from git](#corrupted-statejson) |
| Invalid TODO.json | Medium | [Create minimal version](#invalid-todojson) |
| Schema mismatch | High | [Version upgrade](#schema-mismatch) |
| Network failure | Medium | [Retry with backoff](#network-failure) |
| Push rejected | Medium | [Resolve conflict](#push-rejected) |

---

## Missing AGENT_LINK.md

**Severity:** Critical - Cannot proceed

**Symptoms:**
- File not found at workspace root
- Agent cannot determine boot configuration

**Diagnosis:**
```bash
ls -la AGENT_LINK.md
# ls: cannot access 'AGENT_LINK.md': No such file or directory
```

**Recovery:**
```
ABORT: Cannot proceed without AGENT_LINK.md

This repository is not configured for agent operation.

Options:
1. If this should be an agent workspace:
   - Follow runbooks/workspace-initialization.md
   - Create AGENT_LINK.md from template

2. If this is the wrong repository:
   - Verify you're in the correct workspace
   - Check AGENT_CONFIG.json in project root

3. If AGENT_LINK.md was deleted:
   - Restore from git: git checkout HEAD -- AGENT_LINK.md
   - If not in git history, recreate from template
```

**Prevention:**
- Never delete AGENT_LINK.md
- Include in .gitignore exceptions if using global ignores

---

## Corrupted STATE.json

**Severity:** High - State lost

**Symptoms:**
- JSON parse error
- File contains garbage or partial content
- Unexpected characters

**Diagnosis:**
```bash
cat agent/STATE.json | jq .
# parse error: Invalid numeric literal at line 5, column 10
```

**Recovery:**

### Option 1: Restore from last valid commit
```bash
# Find last good version
git log --oneline agent/STATE.json

# Restore specific version
git checkout <commit-sha> -- agent/STATE.json

# Verify
cat agent/STATE.json | jq .
```

### Option 2: Restore from git stash
```bash
# Check for stashed changes
git stash list

# Apply if relevant
git stash pop
```

### Option 3: Reconstruct from changelog
```bash
# Read recent changelog entries
tail -50 CHANGELOG.md

# Manually reconstruct STATE.json based on:
# - Last known phase
# - Last known intent
# - Recent activity
```

### Option 4: Start fresh (last resort)
```bash
# Only if no recovery possible
cp templates/STATE.template.json agent/STATE.json

# Update with known values
# WARNING: Loses continuity from previous sessions
```

**Prevention:**
- Always validate before commit
- Commit frequently
- Don't edit STATE.json manually

---

## Invalid TODO.json

**Severity:** Medium - Tasks may be lost

**Symptoms:**
- Schema validation fails
- Missing required fields
- Invalid enum values

**Diagnosis:**
```bash
ajv validate -s schemas/todo-schema.json -d agent/TODO.json
# data/tasks/0 must have required property 'priority'
```

**Recovery:**

### For missing fields:
```bash
# Add missing fields with defaults
jq '.tasks |= map(. + {priority: "medium"} | select(.priority))' \
  agent/TODO.json > agent/TODO.json.fixed
mv agent/TODO.json.fixed agent/TODO.json
```

### For invalid values:
```bash
# Fix invalid enum values
jq '.tasks |= map(
  if .status == "wip" then .status = "in-progress"
  elif .status == "complete" then .status = "done"
  else . end
)' agent/TODO.json > agent/TODO.json.fixed
mv agent/TODO.json.fixed agent/TODO.json
```

### For structural issues:
```bash
# Create minimal valid version
cat > agent/TODO.json << 'EOF'
{
  "version": "1.0.0",
  "tasks": [],
  "metadata": {
    "note": "Recreated due to corruption. Check git history for lost tasks."
  }
}
EOF
```

**Prevention:**
- Validate before every commit
- Use schema-aware editors
- Don't manually edit JSON files

---

## Schema Mismatch

**Severity:** High - Version conflict

**Symptoms:**
- Fields present that schema doesn't allow
- Required fields missing in newer schema
- Enum values changed between versions

**Diagnosis:**
```bash
# Check versions
jq .version agent/STATE.json
# "0.9.0"

jq .bootVersion VERSION.json
# "1.0.0"
```

**Recovery:**

### Identify the conflict:
```bash
# Validate and capture errors
ajv validate -s schemas/state-schema.json -d agent/STATE.json 2>&1
```

### Upgrade path (0.9.x → 1.0.x):
```bash
# Add new required fields
jq '. + {
  currentPhase: (.phase // "idle"),
  errors: (.errors // [])
} | del(.phase)' agent/STATE.json > agent/STATE.json.upgraded

# Validate upgraded version
ajv validate -s schemas/state-schema.json -d agent/STATE.json.upgraded

# Apply if valid
mv agent/STATE.json.upgraded agent/STATE.json
```

### Downgrade path (if boot is older):
```
WARNING: Downgrading is not recommended.

Options:
1. Update boot reference in AGENT_LINK.md to newer version
2. Contact repository maintainer for compatibility guidance
```

**Prevention:**
- Pin boot version in AGENT_LINK.md
- Test compatibility before upgrading
- Read VERSION.json changelog before updates

---

## Network Failure

**Severity:** Medium - Temporary

**Symptoms:**
- Cannot fetch boot repository
- Push/pull operations fail
- Timeout errors

**Diagnosis:**
```bash
# Test connectivity
curl -I https://github.com
# curl: (6) Could not resolve host: github.com
```

**Recovery:**

### Retry with exponential backoff:
```bash
for i in 1 2 3 4; do
  git push && break
  echo "Attempt $i failed, waiting $((2**i)) seconds..."
  sleep $((2**i))
done
```

### If persistent:
```
Network recovery not possible.

Options:
1. Check internet connectivity
2. Check GitHub status: https://www.githubstatus.com
3. Work offline and sync later:
   - Continue local work
   - Commit locally
   - Push when connectivity restored
```

**Prevention:**
- Have offline procedures documented
- Commit frequently (smaller pushes recover faster)

---

## Push Rejected

**Severity:** Medium - Conflict

**Symptoms:**
- `git push` fails with non-fast-forward error
- Remote has changes not in local

**Diagnosis:**
```bash
git push
# ! [rejected] main -> main (non-fast-forward)
```

**Recovery:**

### Standard resolution:
```bash
# Fetch remote changes
git fetch origin

# Rebase local changes
git rebase origin/main

# Resolve any conflicts
# ... edit files ...
git add <resolved-files>
git rebase --continue

# Push
git push
```

### If rebase is problematic:
```bash
# Merge instead
git pull origin main

# Resolve conflicts
# ... edit files ...
git add <resolved-files>
git commit -m "Merge remote changes"

# Push
git push
```

### For agent state conflicts:
```
STATE.json or TODO.json conflicts require careful resolution:

1. Keep the version with more recent lastUpdated
2. Merge tasks arrays (dedupe by id)
3. Validate merged result against schema
4. Test before pushing
```

**Prevention:**
- Pull before starting work
- Push frequently
- Don't have multiple agents on same branch

---

## Recovery Checklist

After any recovery:

- [ ] Files parse as valid JSON
- [ ] Schema validation passes
- [ ] Git status is clean
- [ ] Can push to remote
- [ ] Changelog documents recovery action
