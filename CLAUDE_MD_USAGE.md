# Claude.md Configuration System - Usage Guide

## Overview

The claude.md configuration system establishes behavioral standards and quality gates for Claude Code across your organization and projects. It uses a hierarchical configuration approach where organizational defaults can be selectively overridden at the project level.

This guide now includes Critical Behavior hard rules that Claude Code must never break, stricter context management (10–20% headroom), and explicit stop‑the‑line triggers and approval gates.

## Configuration Hierarchy

```
~/.claude/claude.md          (Organizational Defaults + Critical Behavior hard rules)
    ↓
/project/root/claude.md      (Project-Specific Overrides + Critical Behavior section)
```

### How It Works

1. **Organizational Level** (`~/.claude/claude.md`)
   - Sets baseline behavior for ALL projects
   - Enforces organizational quality standards
   - Defines Critical Behavior hard rules (must never be broken)
   - Provides standard templates and communication patterns
   - Cannot be bypassed; only explicit, owner‑approved exceptions may relax hard rules (see Exceptions)

2. **Project Level** (`/project/root/claude.md`)
   - Inherits all organizational defaults
   - Must include a top‑level “Critical Behavior (Project‑Level)” section
   - Can add stricter rules; may propose an exception to relax org hard rules with explicit approval
   - Adds project-specific context (stack, tools, conventions)
   - Must justify any override with rationale and risk mitigation

## Quick Start

### For New Projects

1. Copy the template to your project root:
   ```bash
   cp ~/.claude/claude-project-template.md /path/to/project/claude.md
   ```

2. Fill in the project-specific sections:
   - Stack details
   - Tool configurations
   - Testing requirements
   - CI/CD pipeline specifics

3. Complete the “Critical Behavior (Project‑Level)” section:
   - Add domain‑specific stop‑the‑line triggers (e.g., accessibility, i18n, performance budgets)
   - Add any project‑specific “always ask before” items
   - Reiterate that placeholders and bypassing failing checks are disallowed

3. Document any necessary overrides with clear justification

### For Existing Projects

1. Review the organizational claude.md for new requirements
2. Assess current project compliance
3. Create project claude.md documenting:
   - Current stack and tooling
   - Critical Behavior section (project additions)
   - Any necessary overrides with migration plans
   - Timeline for achieving full compliance

## Key Features

### Critical Behavior (Hard Rules)
- Stop‑the‑line on detected errors or inconsistencies (including preexisting); do not proceed until a human decision is made.
- No assumptions: ask when <90% confident and cite evidence (files:lines) with a recommended default.
- No placeholders/shortcuts: deliver complete, working implementations with tests and docs.
- Verify before acting: double‑check folder structures, route/registry wiring, exports, runtime versions, and docs/ADRs.
- Context management: maintain 10–20% headroom; if the next step risks dropping below, pause and use `/compact` first.
- Do not bypass quality gates: never disable linters/tests or merge with failing checks without an explicit, time‑boxed exception and ticket.
- No destructive operations without explicit approval and backups.
- Always ask before adding deps, making schema/API changes (esp. breaking), data migrations, CI/CD or infra edits, or security/policy changes.

### Stop-the-Line Policy
When Claude Code encounters errors, it will:
1. Immediately stop execution
2. Present a structured analysis
3. Offer options (fix now, defer with ticket, proceed with mitigation)
4. Wait for your decision

**Example Response:**
```
Found preexisting issue: TypeScript compilation errors in 3 files
Impact: Build pipeline blocked
Likely cause: Recent dependency update incompatible types (src/api/client.ts:45)
Options:
  (A) Fix now - Update type definitions (~15 min)
  (B) Defer - Create ticket, pin to previous version
  (C) Proceed with mitigation - Use type assertions temporarily
Recommend (A) because it prevents accumulating tech debt
Approve?
```

#### Common Triggers
- Failing lint/format/type‑check/unit/integration/build steps
- Unresolved DB migrations/schema drift; data‑loss risk
- Missing/invalid environment variables or secrets
- High/Critical vulnerability or license violation in dependencies
- Route/registry mismatches; unmounted handlers; export/import/case sensitivity errors
- Toolchain/runtime drift from documented versions; reproducibility failures
- Security/privacy risks (secrets in code/logs, PII exposure)

### Context Management
When working on long tasks, Claude Code monitors context usage and will proactively suggest compaction. Maintain 10–20% context headroom. If the next step’s plan or patch would reduce headroom below this threshold, pause and use `/compact` before proceeding.

**When to expect this:**
- Long debugging sessions
- Multi-file refactoring
- Complex feature implementations

**How to respond:**
- Use the provided `/compact` prompt template when prompted, or proactively use `/compact` with the template from claude.md.
- Proceed only after confirming the compacted context retains requirements, decisions, edge cases, interfaces, key file paths, and open questions.

### Extreme Planning
Claude Code will create detailed plans before implementation:
- Discovery phase to understand existing code
- Verification of assumptions
- Design decisions with rationale
- Step-by-step implementation plan
- Test coverage planning
- Documentation updates

## Common Scenarios

### Scenario 1: Strict Org Standards, Legacy Project
Your organization requires 80% test coverage, but your legacy project has 30%.

**In project claude.md:**
```markdown
## Overrides to Org Spec

### Test Coverage Requirement
- **What**: Reduced coverage target of 50% (vs 80% org standard)
- **Why**: Legacy codebase with 30% current coverage; gradual improvement plan
- **Risk mitigation**:
  - All NEW code must have 80% coverage
  - Quarterly reviews to increase overall coverage by 5%
  - Critical paths have dedicated integration tests
```

### Scenario 2: Experimental Project
Working on a proof-of-concept that needs rapid iteration.

**In project claude.md:**
```markdown
## Overrides to Org Spec

### Rapid Prototyping Mode
- **What**: Relaxed documentation and test requirements for prototype phase
- **Why**: 2-week spike to validate technical feasibility
- **Risk mitigation**:
  - Code clearly marked as prototype
  - Full rewrite required before production
  - Findings documented in ADR before proceeding
```

### Scenario 3: High-Security Project
Project with stricter requirements than organizational defaults.

**In project claude.md:**
```markdown
## Security/Compliance

### Enhanced Security Requirements
- All dependencies must be approved by security team
- 100% test coverage for authentication/authorization code
- Mandatory security review for all PRs
- No direct database access; all through validated ORM
- Audit logging for all state changes
```

## Template Usage

### Error Escalation Template
Claude Code uses this when encountering blocking issues. You'll see structured problem reports that help you make informed decisions quickly.

### Confidence <90% Template
When Claude Code is uncertain, it will:
- Show what it found (with file references)
- List what's unclear
- Propose a default action
- Ask for your guidance

### /compact Prompt Template
When context is running low:
1. Claude Code will alert you
2. You can use `/compact` command
3. Provide the template prompt from claude.md
4. Claude will optimize context while retaining critical information

### Extreme Plan Template
Used for all non-trivial implementations. Ensures:
- Nothing is overlooked
- Clear acceptance criteria
- Traceable artifacts
- Defined verification methods

### PR Checklist Template
- A ready-to-use checklist is available at `~/.claude/claude-pr-checklist.md`.
- Copy it into your repository as `.github/pull_request_template.md` or `docs/pr-checklist.md`:
  ```bash
  # GitHub PR template (recommended)
  mkdir -p .github && cp ~/.claude/claude-pr-checklist.md .github/pull_request_template.md

  # Or keep as a doc in your repo
  mkdir -p docs && cp ~/.claude/claude-pr-checklist.md docs/pr-checklist.md
  ```
- Ensure it’s kept in sync with your project’s `claude.md` (project-level Critical Behavior) and this usage guide.

## Exceptions
- Critical Behavior rules must not be relaxed without an explicit, owner‑approved exception.
- To request an exception:
  1. Document the proposed relaxation in the project’s `claude.md` under “Overrides to Org Spec”.
  2. Provide business/technical justification, risk mitigation, timebox, and rollback plan.
  3. Obtain approval from the designated owner/maintainer.
  4. Create a tracking ticket and target date to restore full compliance.

## Best Practices

### DO:
- ✅ Keep project claude.md up-to-date with stack changes
- ✅ Ensure the project file begins with “Critical Behavior (Project‑Level)” and lists domain gates and ask-before items
- ✅ Document reasons for any organizational overrides
- ✅ Use the stop-the-line policy to maintain quality
- ✅ Respond to Claude's confidence checks promptly
- ✅ Proactively use /compact on long sessions

### DON'T:
- ❌ Override organizational standards without documentation
- ❌ Ignore error escalations
- ❌ Skip the planning phase for complex tasks
- ❌ Let context headroom drop below 10–20% without compaction
- ❌ Use overrides as permanent solutions

## Troubleshooting

### Claude Code isn't following the standards
1. Verify `~/.claude/claude.md` exists
2. Check for conflicting project-level overrides
3. Ensure markdown formatting is correct
4. Try explicitly referencing the standards in your request
5. Confirm the project file includes the Critical Behavior section and no rule conflicts

### Context keeps overflowing
1. Use /compact more frequently and earlier (maintain 10–20% headroom)
2. Break large tasks into smaller chunks
3. Clear completed work from context
4. Focus on one subsystem at a time

### Too many stop-the-line interruptions
1. Review if project overrides are needed
2. Consider a "cleanup sprint" to address debt
3. Document known issues in project claude.md
4. Set up pre-commit hooks to catch issues earlier

## Maintenance

### Quarterly Review Checklist
- [ ] Review organizational claude.md for updates
- [ ] Assess project override necessity
- [ ] Update project documentation
- [ ] Plan migration for deprecated overrides
- [ ] Share learnings with other teams

### Updating Organizational Standards
1. Propose changes with rationale
2. Get stakeholder buy-in
3. Update `~/.claude/claude.md`
4. Communicate changes to all teams
5. Provide migration timeline for existing projects

## Support

For questions or improvements to the claude.md system:
1. Review this documentation
2. Check project-specific overrides
3. Consult with your team lead
4. Propose improvements via pull request to organizational standards

---
*This configuration system ensures consistent, high-quality code assistance from Claude Code while allowing necessary flexibility for project-specific needs.*
