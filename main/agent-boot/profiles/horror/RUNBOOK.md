# Horror Runbook

## Overview
This runbook defines operational procedures for the horror agent profile, designed for identifying and auditing dangerous patterns, anti-patterns, and potential issues.

## Purpose
Systematically identify "horror stories" - problematic code, configurations, security issues, and architectural anti-patterns that could cause serious problems.

## Startup Procedure

1. Load AUDIT_CHECKLIST.md
2. Initialize issue tracker
3. Set severity thresholds
4. Prepare report templates

## Audit Workflow

### 1. Scope Definition
- Identify target systems/code
- Define audit boundaries
- Set exclusions if any
- Establish baseline

### 2. Discovery Phase
For each checklist category:
1. Scan for indicators
2. Flag potential issues
3. Gather evidence
4. Document location

### 3. Analysis Phase
For each flagged item:
1. Verify issue exists
2. Assess severity
3. Determine impact
4. Identify root cause

### 4. Classification
Assign severity:
- **CRITICAL**: Immediate action required
- **HIGH**: Address within 24 hours
- **MEDIUM**: Address within 1 week
- **LOW**: Address when convenient
- **INFO**: Awareness only

### 5. Reporting
- Generate findings report
- Prioritize by severity
- Include remediation steps
- Provide executive summary

## Detection Patterns

### Security Horrors
- Hardcoded credentials
- SQL injection vectors
- XSS vulnerabilities
- Insecure configurations

### Code Horrors
- Infinite loops
- Memory leaks
- Race conditions
- Unhandled exceptions

### Architecture Horrors
- Circular dependencies
- God objects
- Spaghetti code
- Missing abstractions

### Operations Horrors
- No monitoring
- Missing backups
- No disaster recovery
- Undocumented processes

## Escalation Matrix

| Severity | Response Time | Escalate To |
|----------|--------------|-------------|
| CRITICAL | Immediate | Security Team |
| HIGH | 4 hours | Team Lead |
| MEDIUM | 24 hours | Project Owner |
| LOW | 1 week | Backlog |
