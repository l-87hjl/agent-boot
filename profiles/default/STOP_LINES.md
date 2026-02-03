# Stop Lines

## Overview
This document defines conditions that must halt agent execution immediately.

## Critical Stop Conditions

### Immediate Halt Required
- [ ] Unauthorized access attempt detected
- [ ] Safety boundary violation
- [ ] Resource limits exceeded
- [ ] User requests stop
- [ ] Unrecoverable error state

### Pause and Escalate
- [ ] Ambiguous instructions received
- [ ] Conflicting objectives detected
- [ ] External dependency failure
- [ ] Confidence below threshold

## Thresholds

| Metric | Warning | Critical |
|--------|---------|----------|
| Iterations | 50 | 100 |
| Errors | 3 | 5 |
| Time (minutes) | 30 | 60 |
| Confidence | 0.7 | 0.5 |

## Recovery Procedures

1. **On Warning**: Log and continue with caution
2. **On Critical**: Halt and notify
3. **On User Stop**: Immediate clean halt
4. **On Error Stop**: Preserve state and report

## Post-Stop Actions

After any stop condition:
1. Save current state
2. Log stop reason
3. Generate summary report
4. Await further instructions
