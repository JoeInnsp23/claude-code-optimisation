# PR Checklist (Claude Code Quality Gates)

Use this checklist on every PR to enforce organizational standards and project rules. Do not merge unless all mandatory items are satisfied or an approved exception is attached.

## Critical Behavior Compliance (Hard Rules)
- [ ] Stop-the-line policy respected; no known blocking errors ignored
- [ ] No assumptions made below 90% confidence without explicit Q&A and evidence
- [ ] No placeholders/stubs/TODOs; implementation is complete and working
- [ ] Folder structure, routes/registries, and exports verified against docs/code
- [ ] Context headroom maintained at 10–20%; `/compact` used when needed

## Planning & Scope
- [ ] Detailed plan executed; steps marked complete with any deltas/risks noted
- [ ] Change is minimal and atomic; one logical concern per PR
- [ ] Mechanical changes (format/rename) isolated from behavioral changes

## Tests & Quality Checks
- [ ] Unit tests added/updated for all new/changed behavior
- [ ] Integration tests added/updated where meaningful
- [ ] Lint/format clean; type-check passes (if applicable)
- [ ] Build passes; smoke run validated (if relevant)

## Security & Compliance
- [ ] No secrets/PII in code, logs, or examples
- [ ] Dependencies scanned; no High/Critical vulns or license violations

## Docs & Operability
- [ ] README/usage updated; ADR added/updated for significant decisions
- [ ] Migrations/rollback steps documented and tested (if applicable)

## Approvals & Exceptions
- [ ] “Always ask before” actions approved (deps, schema/API changes, migrations, CI/infra/security edits)
- [ ] Any exceptions documented with ticket, timebox, and rollback plan

## Links
- Organizational spec: `~/.claude/claude.md`
- Project spec: `./claude.md`
- Usage guide: `~/.claude/CLAUDE_MD_USAGE.md`

