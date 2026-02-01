# Agent Loop

## Overview
This document defines the execution loop that governs agent behavior.

## Main Loop Structure

```
WHILE active:
    1. OBSERVE  - Gather current state and context
    2. ORIENT   - Analyze situation and options
    3. DECIDE   - Select appropriate action
    4. ACT      - Execute chosen action
    5. REFLECT  - Evaluate results and update state
```

## Phase Details

### 1. OBSERVE
- Read current state from STATE.md
- Check for new inputs or triggers
- Gather relevant context

### 2. ORIENT
- Analyze current situation
- Identify available options
- Consider constraints and rules

### 3. DECIDE
- Evaluate options against objectives
- Check against STOP_LINES.md
- Select optimal action

### 4. ACT
- Execute selected action
- Log action to CHANGELOG.md
- Update TODO.md as needed

### 5. REFLECT
- Evaluate action results
- Update STATE.md
- Determine if objectives met

## Loop Termination

Exit loop when:
- All objectives completed
- Stop condition triggered
- Error requires escalation
- Manual intervention requested
