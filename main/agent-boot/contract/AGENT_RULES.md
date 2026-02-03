# Agent Rules Contract

Version: 1.0.0

These rules are **mandatory** for all agents operating in workspaces that reference this boot repository.

---

## Rule 1: Entry Point

**Always read `AGENT_LINK.md` first from the workspace repository.**

- This file defines the boot contract version and memory file locations
- If `AGENT_LINK.md` is missing, abort immediately
- Parse all configuration before taking any action

## Rule 2: Read-Only Boot

**Never write to the agent-boot repository.**

- This repository is enforced read-only via `repo-bridge` (`READ_ONLY_REPOS`)
- All state, todos, and changelogs live in the workspace repository
- Templates and schemas are reference-only; copy to workspace if modification needed

## Rule 3: Schema Validation

**Validate state files against schemas before any action.**

- `agent/STATE.json` must validate against `schemas/state-schema.json`
- `agent/TODO.json` must validate against `schemas/todo-schema.json`
- Invalid files must trigger error recovery, not silent failures

## Rule 4: Changelog Discipline

**Update `CHANGELOG.md` on every workspace commit.**

- Each commit must have a corresponding changelog entry
- Format: `## YYYY-MM-DD` header with bullet points
- Include: what changed, why, any relevant context

## Rule 5: Fail Fast

**Abort if context is incomplete.**

Conditions that require immediate abort:
- Missing required files (`AGENT_LINK.md`, `STATE.json`)
- Invalid JSON (parse errors)
- Schema validation failures
- Version mismatches between boot and workspace

Do not attempt to "work around" missing context. Surface errors clearly.

## Rule 6: Version Compatibility

**Boot repository version must match workspace expectations.**

- Workspace declares expected boot version in `AGENT_LINK.md`
- Agent must verify `VERSION.json` matches before proceeding
- Version format: `MAJOR.MINOR.PATCH` (semver)
- Compatible versions: same MAJOR, MINOR within range

---

## Enforcement

These rules are not suggestions. Violations indicate:
1. Bug in agent implementation
2. Corrupted workspace state
3. Version mismatch requiring upgrade

When in doubt, fail loudly and request human intervention.
