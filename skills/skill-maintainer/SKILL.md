---
name: skill-maintainer
description: Check, edit, and verify skills in this personal skill library to keep them sharp. Always triggers immediately after skill-creator finishes building or editing a skill, no separate request needed, this is a mandatory extension step of that workflow, not an optional follow-up. Also trigger on every other write to a skill file: after any manual edit to an existing skill, or whenever someone asks to check, audit, review, or clean up a specific skill. Works on one named skill at a time, not a full-repo sweep. This skill never builds skill content from scratch and never runs output-quality eval loops or benchmarking, that's skill-creator's job, but every skill skill-creator produces still passes through this checklist before it's considered finished.
---

# Skill Maintainer

This skill checks a skill in this personal library against a fixed quality checklist, reports what it finds, and proposes fixes for approval before editing anything.

## Relationship to skill-creator

skill-creator builds: drafts new skill content, runs test-and-eval loops, benchmarks output quality. This skill checks: runs immediately after skill-creator finishes a new skill, and on every later edit to any skill, regardless of who or what made the edit. Building and checking are different jobs, but checking is not optional just because a skill is brand new.

Treat this as a chained extension of skill-creator, not a separate optional pass: skill-creator's job isn't done until this checklist has run, automatically, without being asked separately.

## Step 0: Get the file

Read the target skill's SKILL.md from the local synced copy at `/mnt/skills/user/<name>/SKILL.md`, plus any bundled reference files it points to, and its sibling `decisions.md` if one exists. If the task needs an actual GitHub action (pushing a fix, syncing to the repo), hand that off to the github-skills-access skill rather than fetching repo credentials here.

## Step 1: Classify before checking anything else

**Source.** Is this an external/community skill, copied in from someone else's public repo, or personally authored?
- External: stop here. Report that it's external and out of scope for editing. These get added verbatim and stay verbatim.
- Personal: continue.

**Type.** Capability Uplift or Encoded Preference?
- Capability Uplift: gives Claude an ability it didn't have before (calls an API, runs a script, touches a tool it couldn't use unprompted). Quality bar is mechanical: does it actually work, is tool access scoped correctly.
- Encoded Preference: Claude already knows how to do the underlying task (write a post, draft an email), the skill encodes the user's specific way of doing it. Quality bar is voice: does it sound like them, does it match the relevant context file.

This decides which checks below actually matter for this particular skill.

## Step 2: Run the checklist

**Description**
- Third person, not "I can help with..."
- States both what the skill does and when to trigger it.
- Specific enough to route correctly, a little pushy rather than vague.
- No collision with another skill's territory. Check especially against the content-writer cluster: social-post-writer, instagram-story-writer, reel-script-writer, blogpost-writer.

**Structure**
- One job per skill. If it does several unrelated things, flag it for splitting.
- Lean body. If it has grown long, detail should live in a reference file, not bloat the main SKILL.md.
- Frontmatter is clean, no leftover or unused fields.

**Rule framing**
- Flag bare ALL-CAPS MUST, NEVER, ALWAYS with no reasoning attached. A rule plus the why generalizes better than a bare command and lets Claude handle edge cases the skill didn't spell out.

**Tool access** (Capability Uplift skills only)
- If the skill runs scripts or calls external services, check whether `allowed-tools` should scope it down rather than leaving default full access.

**Tone** (Encoded Preference skills, client-facing only)
- Spot check against `context/szm/context_studio_klantcommunicatie.md`. Does it still match the established voice and scope rules.

**Memory & personal-detail leakage**
- Flag personal facts (names, hobbies, employers, past jobs, biographical detail) used as illustrative examples in the skill text. Methodology and instruction text should use generic placeholders, never real specifics, regardless of whether the fact is true.
- Flag hardcoded personal infrastructure detail (a file ID, account name, specific path, credential location) the skill needs in order to function. These come from memory, not from the skill's own logic, and they go stale silently the moment the underlying setup changes. Recommend the skill ask for the current value or point to a stable, named config location, instead of hardcoding the literal value into the instructions.

**Staleness**
- Does the skill reflect how the user actually works now, or has their process moved on since it was written. If unsure, ask rather than assume.

**Decisions log**
- Check whether SKILL.md bakes in a time/capability-contingent fact, a statement about what a tool currently can or can't do, directly into the instructions.
- If yes and no sibling `decisions.md` exists: flag it for extraction.
- If `decisions.md` already exists: confirm entries still follow the format `[date last checked]: [reason for decision]`, and confirm SKILL.md still has its one-line pointer to it.

## Step 3: Decide the kind of fix

Match the issue to the right fix. Don't default to rewriting the whole file for everything:
- Wrong order or unclear step → edit SKILL.md directly.
- Missing context the skill needs but doesn't have → add a reference file.
- A mistake that keeps recurring → add a rule, with the reasoning behind it.
- Time/capability fact baked into SKILL.md → extract to a sibling `decisions.md`, leave a one-line pointer in SKILL.md, don't auto-load it.

## Step 4: Report and propose

Output a short pass/flag list, one line per checklist item. For every flag, name the specific fix you'd make. Do not rewrite anything yet.

Wait for approval before editing the file. When editing, make surgical changes only, limited to what was flagged. Don't touch parts of the skill that weren't called out, even if you notice something else along the way, just mention it separately.

## When to hand off instead

If the real problem is output quality rather than structure or voice, for example the skill is well-built but the results it produces aren't landing, that's a job for skill-creator's test-and-eval loop, not this skill. Say so and point there instead of trying to fix it blind.
