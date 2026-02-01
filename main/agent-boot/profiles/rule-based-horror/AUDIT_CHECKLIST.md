# Audit Checklist

## Security

### Authentication & Authorization
- [ ] Hardcoded passwords or API keys
- [ ] Default credentials in use
- [ ] Missing authentication on endpoints
- [ ] Broken access control
- [ ] Session management issues

### Data Protection
- [ ] Sensitive data in logs
- [ ] Unencrypted data at rest
- [ ] Unencrypted data in transit
- [ ] PII exposure risks
- [ ] Missing data validation

### Injection Vulnerabilities
- [ ] SQL injection points
- [ ] Command injection risks
- [ ] XSS vulnerabilities
- [ ] LDAP injection
- [ ] Path traversal

## Code Quality

### Error Handling
- [ ] Empty catch blocks
- [ ] Swallowed exceptions
- [ ] Missing error handling
- [ ] Unclear error messages
- [ ] Error information leakage

### Resource Management
- [ ] Unclosed connections
- [ ] Memory leaks
- [ ] File handle leaks
- [ ] Thread pool exhaustion
- [ ] Missing timeouts

### Logic Issues
- [ ] Off-by-one errors
- [ ] Race conditions
- [ ] Deadlock potential
- [ ] Infinite loops
- [ ] Null pointer risks

## Architecture

### Design Problems
- [ ] Circular dependencies
- [ ] God classes/modules
- [ ] Excessive coupling
- [ ] Missing abstraction layers
- [ ] Violation of SOLID principles

### Scalability Issues
- [ ] N+1 query patterns
- [ ] Missing caching
- [ ] Blocking operations
- [ ] Single points of failure
- [ ] Missing load handling

## Operations

### Observability
- [ ] Missing logging
- [ ] No metrics collection
- [ ] Missing alerting
- [ ] No distributed tracing
- [ ] Insufficient dashboards

### Reliability
- [ ] No backup strategy
- [ ] Missing disaster recovery
- [ ] No failover mechanism
- [ ] Untested recovery procedures
- [ ] Missing runbooks

### Configuration
- [ ] Secrets in source control
- [ ] Environment-specific hardcoding
- [ ] Missing configuration validation
- [ ] No configuration documentation

## Severity Scoring

| Finding | Base Score | Multipliers |
|---------|------------|-------------|
| Security - Critical | 10 | x1.5 if exploitable |
| Security - High | 7 | x1.3 if external facing |
| Code - Critical | 8 | x1.2 if in hot path |
| Code - High | 5 | x1.1 if frequently used |
| Architecture | 6 | x1.4 if core component |
| Operations | 5 | x1.5 if no monitoring |
