---
name: instruction-maintainer
description: Create, check, verify, and maintain high quality instruction files across every surface where Claude's behavior gets configured: claude.ai global preferences (Settings > Profile), claude.ai project instructions (per-project custom instructions), and CLAUDE.md files in Claude Code (both user-level and project-level). Use this skill whenever the user wants to write, review, or audit any of these, asks whether a preference or CLAUDE.md is "any good," mentions preferences growing unwieldy or contradictory, shares CLAUDE.md content, sets up a new project, or asks where a piece of guidance should live. Trigger also when deciding whether something belongs in a global preference vs a project instruction vs CLAUDE.md vs a rule, skill, hook, or linter config.
---

# Instruction Maintainer

Same underlying problem shows up in four different places: bloat, duplication, the wrong kind of content in the wrong layer, and stale guidance nobody revisits. This skill applies one consistent set of rules across all four, and routes anything procedural out to a skill, which works the same way on claude.ai and in Claude Code.

---

## The four surfaces

| Surface | Where it lives | Scope | Loaded |
|---|---|---|---|
| Claude.ai preferences | Settings > Profile > Preferences | Every chat, every project, on claude.ai | Always, every conversation |
| Claude.ai project instructions | Project settings, per project | That project only | Every chat inside that project |
| CLAUDE.md, user-level | `~/.claude/CLAUDE.md` | Every Claude Code session, any repo | Session start |
| CLAUDE.md, project-level | `<repo>/CLAUDE.md` | That repo only | Session start |

## Core rules, apply to all four

- Keep it factual and contextual, not procedural. Multi-step workflows belong in a skill.
- Manually craft each line. Never bulk-generate a preferences page or CLAUDE.md from a transcript dump, unreviewed content carries noise.
- One authoritative location per rule. If the same instruction shows up in two surfaces, decide which one owns it and remove it from the other.
- Document actual recurring patterns, not hypothetical ones. A rule earns its place by having mattered more than once.
- Cap length per surface (see surface-specific notes below). When a surface grows past its cap, split content out rather than letting it run.

## Before adding a new line, anywhere

1. Does this apply across most of what happens on this surface? If it's narrow, it's a skill instead.
2. Does existing guidance already cover this? If yes, consolidate, don't add a second copy elsewhere.
3. Is this really a fact, or is it a step-by-step process wearing a fact's clothing? Processes go in skills.
4. Will this go stale soon (a code snippet, a tool version, a contact detail)? If yes, reference it instead of embedding it.

---

## Surface: claude.ai preferences (global)

Two kinds, treated very differently:

- **Behavioral preferences**: how Claude should act (format, tone, tool use, response length). These apply broadly whenever relevant to the task.
- **Contextual preferences**: background about the person (profession, interests, hobbies). These apply narrowly: only when the query explicitly touches that context, the person asks for personalization, or the query sits squarely in that stated expertise.

**The specific failure mode to audit for**: a contextual fact (DJ background, Genshin Impact, wine industry, retail experience) quietly leaking into an unrelated technical answer. When reviewing this surface, check every contextual preference against: would this show up in a CSS debugging session, an email draft, a coding answer? If yes, flag it as too broadly worded.

## Surface: claude.ai project instructions (per project)

Same factual-not-procedural rule as CLAUDE.md. Scoped to one project's context and conventions, not the steps for doing a task within it. Steps still go in a skill.

This surface used to be project-instruction-writer's job, now covered here directly. See `decisions.md` for why, and when to revisit.

## Surface: CLAUDE.md (Claude Code, user-level and project-level)

### Hard cap and where things actually belong

- 200 lines hard max, 100-150 ideal. (Why 200 over the looser numbers some sources use: `decisions.md`.)
- File references over code snippets: a `path/to/file:line` reference stays accurate, an embedded code block goes stale the moment the real code changes.

| If it's... | It belongs in... | Why |
|---|---|---|
| A fact true for the whole session (build command, folder layout, naming convention) | CLAUDE.md | Loaded once, cached for the session |
| A constraint specific to one part of the codebase | a path-scoped rule in `.claude/rules/` | Only loads when that folder is touched |
| A multi-step procedure | a skill in `.claude/skills/` | Loads in full only when invoked |
| Something narrow, under ~20% of tasks | a skill, not CLAUDE.md | Most sessions never need it |
| A code style or formatting convention | a linter/formatter config | Tools enforce deterministically, text doesn't |
| Something that must happen every time, no exceptions | a hook in `settings.json` | Hooks are deterministic |
| A real "never do this" rule | a hook (PreToolUse, exit code 2) or managed settings | Prompted instructions get followed most of the time, not all of the time |
| A side task you don't need to watch happen | a subagent in `.claude/agents/` | Runs in its own context, only the summary returns |
| A personal habit, not a team norm | the user-level CLAUDE.md, not the project one | Project file is for things every contributor needs |

### Default behavior checklist (Karpathy method)

If a project's CLAUDE.md has no behavior section yet, propose this:

1. Think before coding: state assumptions, surface tradeoffs, don't silently pick an interpretation.
2. Simplicity first: minimum that solves the problem, nothing speculative.
3. Surgical changes: touch only what's asked.
4. Goal-driven execution: define done before starting, verify against it.

### Self-learning loop

After a correction, while it's still in context, say: "reflect, abstract, generalize, add to CLAUDE.md." This turns one mistake into one generalized rule, not a dated note bolted onto the file.

### Audit checklist

- Line count over 200, hard flag. Over 150, worth a mention.
- Duplicate rules stated under more than one heading.
- Accumulating hotfixes: dated one-off notes ("note 2026-03-12: remember to check X") that should have been generalized or dropped.
- Embedded code blocks that mirror something living in the actual codebase.
- Reads like an index, or like a manual with everything inlined.

---

## Reviewing any surface

When asked to check or audit one, don't describe the rules in the abstract. Point at specific lines and say where each one should move, using the tables above for that surface.

## Drafting new content for any surface

Default to short. Ask only for what isn't already visible in the conversation or the repo. For deeper detail on any surface (the full Anthropic instruction-method table, the recommended metrics, Nate Herk's framework structures, why each anti-pattern fails in practice): see `references/best-practices.md`. Source links for all of it: `references/sources.md`.

---

## Updating this skill

Say: "update the instruction-maintainer skill with this rule: [change]." On every correction or new recurring pattern not covered here, propose an update.

## Changelog

- v1: created June 2026 as `claude-md-quality`, based on Anthropic's "Steering Claude Code" post, the Karpathy method, Nate Herk's WAT/AIS-OS research, and the smithery.ai claudemd-maintainer skill.
- v2: renamed to `instruction-maintainer`, scope widened from CLAUDE.md only to all four configuration surfaces (claude.ai global preferences, claude.ai project instructions, CLAUDE.md user-level, CLAUDE.md project-level).
