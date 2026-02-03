# Loop Protocol

The work cycle that governs agent operation.

---

## The Loop

```
┌─────────────────────────────────────────────────────────────────┐
│  1. READ  →  2. PLAN  →  3. ACT  →  4. VERIFY  →  5. PERSIST   │
│     ↑                                                    │      │
│     └────────────────────────────────────────────────────┘      │
└─────────────────────────────────────────────────────────────────┘
```

Every task follows this cycle. Every session is a series of cycles.

---

## Step 1: READ

**Load all relevant context before doing anything.**

Read in this order:
1. `STATE.json` - Where did we leave off?
2. `TODO.json` - What tasks exist?
3. Relevant source files - What are we working with?

Questions to answer:
- What was the last action taken?
- What task am I working on?
- What files are involved?

**Do not proceed until you understand the current state.**

---

## Step 2: PLAN

**Decide what to do before doing it.**

Planning checklist:
- [ ] What is the goal of this action?
- [ ] What files will be modified?
- [ ] What could go wrong?
- [ ] How will I verify success?

Output of planning:
- Clear statement of intent
- List of files to modify
- Expected outcome

**Do not proceed until you have a plan.**

---

## Step 3: ACT

**Execute the plan, one step at a time.**

Execution rules:
- Take one action at a time
- Check results after each action
- Stop immediately if something unexpected happens
- Document what you're doing as you do it

If the action fails:
- Do not retry blindly
- Document the failure
- Return to PLAN with new information

**Do not proceed to VERIFY until the action is complete.**

---

## Step 4: VERIFY

**Confirm the action succeeded.**

Verification checklist:
- [ ] Did the expected files change?
- [ ] Are there any error messages?
- [ ] Does the result match the plan?
- [ ] Are there any side effects?

If verification fails:
- Document what went wrong
- Do not mark task as complete
- Return to PLAN or escalate to user

**Do not proceed to PERSIST if verification failed.**

---

## Step 5: PERSIST

**Save state and document what happened.**

Persistence checklist:
- [ ] Update STATE.json with new state
- [ ] Update TODO.json if task status changed
- [ ] Add CHANGELOG entry
- [ ] Commit changes with clear message

Commit message format:
```
<type>: <brief description>

- What changed
- Why it changed
- Any relevant context
```

**The cycle is not complete until state is persisted.**

---

## Cycle Frequency

- **Minimum:** One full cycle per meaningful action
- **Maximum:** One full cycle per small atomic change

When in doubt, complete the cycle more frequently. It's better to persist too often than to lose state.

---

## Breaking the Loop

The only valid reasons to break the loop:
1. User requests stop
2. Critical error that cannot be handled
3. Session timeout

If breaking the loop:
1. Persist whatever state you can
2. Document why the loop was broken
3. Set `nextAction` in STATE.json for next session

---

## Loop Violations

These are violations of the loop protocol:
- Acting without reading context
- Acting without a plan
- Persisting without verification
- Marking tasks complete without verification
- Skipping CHANGELOG updates

If you catch yourself violating the loop, STOP and restart the current cycle.
