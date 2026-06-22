# Maintain CLAUDE.md

Keep CLAUDE.md files accurate, current, and useful.

## When to use this skill

Invoke `/maintain-claude-md` when:
- Starting work in a new repo with no CLAUDE.md
- The existing CLAUDE.md feels stale or incomplete
- You've learned something new about a project worth documenting
- Asked to update or refresh project docs

## What belongs in CLAUDE.md

CLAUDE.md is a compact reference for Claude — not a README or onboarding guide. It should contain only what Claude *can't* derive from reading the code:

**Include:**
- Build, test, lint, and run commands (exact invocations)
- Non-obvious architectural decisions or constraints
- Project-specific conventions (naming, file layout, patterns)
- Known gotchas, pitfalls, or things that would surprise a new contributor
- External systems and where to find them (CI, staging URL, key config files)
- Team workflow rules (PR process, branching, etc.)

**Exclude:**
- Things obvious from reading the code or package.json
- General best practices that aren't project-specific
- Descriptions of what files/directories do (derivable from structure)
- Lengthy explanations — one tight line beats a paragraph

## Process

1. **Discover the project** — read `package.json`, `Makefile`, `pyproject.toml`, `Cargo.toml`, or whatever applies; scan the README; run `ls` at root level
2. **Read any existing CLAUDE.md** — note what's there, what's missing, what's wrong
3. **Draft updates** — focus on commands and non-obvious facts
4. **Write concisely** — bullet points, exact commands, no prose padding
5. **Confirm before writing** — show the diff to the user before saving if the changes are substantial

## Format

```markdown
# CLAUDE.md

## Commands
- Build: `<exact command>`
- Test: `<exact command>`
- Lint: `<exact command>`
- Dev server: `<exact command>`

## Architecture
- <key decision or constraint>

## Conventions
- <project-specific pattern>

## Gotchas
- <thing that would waste time if unknown>
```

Sections are optional — only include what's actually useful. Keep the whole file under ~50 lines; if it grows longer, cut the lowest-value lines.
