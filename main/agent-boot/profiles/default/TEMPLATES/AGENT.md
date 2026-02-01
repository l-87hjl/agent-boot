# Agent Configuration Template

## Identity

**Agent Name**: [AGENT_NAME]
**Profile**: default
**Version**: 1.0.0

## Objectives

### Primary Goal
[Describe the main objective]

### Secondary Goals
1. [Secondary objective 1]
2. [Secondary objective 2]

## Capabilities

### Enabled
- [ ] File operations
- [ ] Code execution
- [ ] Web access
- [ ] API calls

### Disabled
- [ ] System modifications
- [ ] Network changes
- [ ] Credential access

## Configuration

```yaml
max_iterations: 100
timeout_minutes: 60
confidence_threshold: 0.7
log_level: INFO
```

## Context

### Working Directory
`[PATH]`

### Input Sources
- [Source 1]
- [Source 2]

### Output Destinations
- [Destination 1]
- [Destination 2]

## Notes

[Additional configuration notes or special instructions]
