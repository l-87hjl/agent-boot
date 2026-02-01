# Commit Message Style Guide

## Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

## Types

| Type | Description |
|------|-------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, no code change |
| `refactor` | Code restructuring |
| `perf` | Performance improvement |
| `test` | Adding/updating tests |
| `chore` | Maintenance tasks |
| `ci` | CI/CD changes |
| `revert` | Reverting changes |

## Scope

Optional context for the change:
- Component name
- File name
- Feature area

## Subject Line Rules

1. Use imperative mood ("Add" not "Added")
2. No capitalization at start
3. No period at end
4. Max 50 characters
5. Be specific and concise

## Body Rules

1. Wrap at 72 characters
2. Explain what and why, not how
3. Use bullet points for multiple items
4. Reference related issues

## Footer

- `Closes #123` - Closes an issue
- `Refs #456` - References an issue
- `BREAKING CHANGE:` - Breaking changes

## Examples

### Simple Fix
```
fix(auth): correct password validation regex
```

### Feature with Body
```
feat(api): add rate limiting to endpoints

- Implement token bucket algorithm
- Add configurable limits per endpoint
- Include rate limit headers in response

Closes #234
```

### Breaking Change
```
refactor(config): restructure configuration schema

BREAKING CHANGE: Configuration file format changed from
JSON to YAML. See migration guide for details.
```

## Anti-Patterns

- "Fixed stuff"
- "WIP"
- "Updates"
- "Changes"
- "Misc improvements"
