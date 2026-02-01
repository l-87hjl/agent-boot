# File Naming Conventions

## General Rules

1. Use lowercase for all file names
2. Use underscores `_` or hyphens `-` as separators (be consistent)
3. Be descriptive but concise
4. Avoid special characters
5. Include appropriate extensions

## By File Type

### Markdown Documentation
```
UPPERCASE.md     - Core agent documents (AGENT_RULES.md, LOOP.md)
lowercase.md     - Supporting documents (commit_message_style.md)
```

### Configuration Files
```
config.yaml      - Main configuration
config.dev.yaml  - Environment-specific
.env.example     - Environment template
```

### Code Files
```
snake_case.py    - Python modules
camelCase.js     - JavaScript modules
PascalCase.ts    - TypeScript classes
kebab-case.vue   - Vue components
```

### Templates
```
TEMPLATE_NAME.md - Template files (uppercase)
_partial.md      - Partial/included files (underscore prefix)
```

## Directory Naming

```
lowercase/       - Standard directories
UPPERCASE/       - Special directories (TEMPLATES/)
.hidden/         - Hidden/config directories
```

## Version Indicators

```
document_v1.md        - Major version only
document_v1.2.md      - Major.minor version
document_20240115.md  - Date-based version
```

## Patterns to Avoid

| Bad | Good | Reason |
|-----|------|--------|
| `My Document.md` | `my_document.md` | Spaces cause issues |
| `finalFINAL.md` | `document_v2.md` | Use versioning |
| `doc1.md` | `user_guide.md` | Be descriptive |
| `TEMP_delete_later.md` | (delete it) | No temp files |
| `file(1).md` | `file_backup.md` | No special chars |

## Naming by Purpose

### Agent Core Files
- `AGENT_RULES.md` - Agent behavior rules
- `LOOP.md` - Execution loop definition
- `STOP_LINES.md` - Termination conditions
- `STATE.md` - Current state tracking
- `TODO.md` - Task tracking
- `CHANGELOG.md` - Action history

### Profile Files
- `RUNBOOK.md` - Operational procedures
- `RUBRIC_SCHEMA.md` - Evaluation schema
- `AUDIT_CHECKLIST.md` - Audit criteria

### Shared Files
- `commit_message_style.md` - Git conventions
- `file_naming.md` - This document
