# Loop Protocol

Version: 1.0.0

The 7-step execution cycle that every agent must follow.

---

## Overview

```
┌─────────────────────────────────────────────────────────────────┐
│  1. READ ENTRY  →  2. LOAD BOOT  →  3. REHYDRATE STATE         │
│        ↓                                    ↓                   │
│  7. PERSIST    ←   6. ACT        ←   5. PLAN   ←  4. VALIDATE  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Step 1: Read Entry

**Purpose:** Locate and parse the workspace's agent configuration.

**Required Files:**
- `AGENT_LINK.md` in workspace root

**Actions:**
1. Read `AGENT_LINK.md`
2. Extract boot repository reference (owner, repo, ref, version)
3. Extract memory file paths (STATE.json, TODO.json, CHANGELOG.md)

**Validation:**
- File exists and is readable
- All required fields present
- Boot version specified

**Failure Modes:**
| Condition | Action |
|-----------|--------|
| `AGENT_LINK.md` missing | ABORT - Cannot proceed without entry point |
| Parse error | ABORT - Malformed configuration |
| Missing boot reference | ABORT - No boot source defined |

---

## Step 2: Load Boot

**Purpose:** Fetch and validate the boot contract.

**Required Files:**
- `VERSION.json` from boot repo
- `contract/AGENT_RULES.md` from boot repo
- `schemas/*.json` from boot repo

**Actions:**
1. Fetch boot repository at specified ref
2. Read `VERSION.json`
3. Verify version compatibility with workspace expectations

**Validation:**
- Boot repo accessible
- Version matches workspace expectation (semver compatible)
- All schema files present

**Failure Modes:**
| Condition | Action |
|-----------|--------|
| Boot repo inaccessible | ABORT - Network/permission error |
| Version mismatch | ABORT - Upgrade required |
| Missing schemas | ABORT - Incomplete boot repo |

---

## Step 3: Rehydrate State

**Purpose:** Load persistent state from workspace repository.

**Required Files:**
- `agent/STATE.json`
- `agent/TODO.json`
- `CHANGELOG.md`

**Actions:**
1. Read `STATE.json` and parse
2. Read `TODO.json` and parse
3. Read `CHANGELOG.md` for recent context
4. Reconstruct agent context from persisted data

**Validation:**
- JSON files parse successfully
- Required fields present

**Failure Modes:**
| Condition | Action |
|-----------|--------|
| `STATE.json` missing | Initialize from template (first run) |
| `STATE.json` corrupted | Restore from last valid git commit |
| `TODO.json` missing | Initialize from template |
| `TODO.json` corrupted | Create minimal valid version |

---

## Step 4: Validate

**Purpose:** Ensure all state conforms to schemas.

**Required Files:**
- `schemas/state-schema.json`
- `schemas/todo-schema.json`

**Actions:**
1. Validate `STATE.json` against `state-schema.json`
2. Validate `TODO.json` against `todo-schema.json`
3. Check cross-references (task IDs, dependencies)

**Validation:**
- All required fields present
- Field types match schema
- Enum values valid
- No orphaned references

**Failure Modes:**
| Condition | Action |
|-----------|--------|
| Schema validation fails | Log specific errors, attempt recovery |
| Unrecoverable validation | ABORT - Manual intervention needed |

---

## Step 5: Plan

**Purpose:** Determine next actions based on current state.

**Required Context:**
- Validated `STATE.json` (currentPhase, nextIntent)
- Validated `TODO.json` (pending tasks, priorities)
- User request (if any)

**Actions:**
1. Check `nextIntent` from previous session
2. Review pending tasks by priority
3. Incorporate user request
4. Determine concrete next steps
5. Update `STATE.json` with plan

**Decision Tree:**
```
Has user request?
├─ Yes → Prioritize user request, update TODO
└─ No → Check nextIntent
         ├─ Has nextIntent → Continue from previous plan
         └─ No nextIntent → Pick highest priority pending task
```

**Failure Modes:**
| Condition | Action |
|-----------|--------|
| No tasks, no intent | Prompt user for direction |
| Conflicting priorities | Surface conflict, ask for clarification |

---

## Step 6: Act

**Purpose:** Execute the planned actions.

**Actions:**
1. Execute planned steps
2. Track progress in real-time
3. Update `TODO.json` task statuses
4. Capture outputs and artifacts
5. Log significant events

**Guardrails:**
- Respect `STOP_LINES.md` conditions
- Check for errors after each significant action
- Maintain audit trail

**Failure Modes:**
| Condition | Action |
|-----------|--------|
| Action fails | Log error, update STATE with failure |
| Stop condition met | Halt gracefully, persist state |
| Unexpected error | Capture context, fail safely |

---

## Step 7: Persist

**Purpose:** Save all state changes to workspace repository.

**Required Files to Update:**
- `agent/STATE.json`
- `agent/TODO.json`
- `CHANGELOG.md`

**Actions:**
1. Update `STATE.json` with:
   - `lastUpdated` timestamp
   - `lastCommitSha` (after commit)
   - `currentPhase` reflecting completion
   - `nextIntent` for next session
2. Update `TODO.json` with task status changes
3. Append to `CHANGELOG.md`
4. Git add, commit, push

**Commit Message Format:**
```
agent: <brief description>

- Updated STATE: <changes>
- Updated TODO: <changes>
- Session summary: <what was accomplished>
```

**Validation:**
- All JSON still validates against schemas
- Commit succeeds
- Push succeeds

**Failure Modes:**
| Condition | Action |
|-----------|--------|
| Commit fails | Retry with conflict resolution |
| Push fails | Retry with exponential backoff |
| Persistent push failure | Log state locally, alert user |

---

## State Decision Tree

```
STATE.json Status?
├─ Missing
│   └─ Is this first run?
│       ├─ Yes → Initialize from template
│       └─ No → ERROR: State lost, check git history
├─ Present but invalid JSON
│   └─ Restore from last valid commit
├─ Present but fails schema
│   └─ Identify specific failures
│       ├─ Missing required field → Add with default
│       └─ Wrong type → ERROR: Manual fix needed
└─ Present and valid
    └─ Proceed to PLAN step
```

---

## Cycle Timing

Each cycle should:
- Complete all 7 steps
- Persist state even on failure (capture error state)
- Be idempotent (re-running from same state = same outcome)
