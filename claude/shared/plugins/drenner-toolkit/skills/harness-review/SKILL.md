---
name: harness-review
description: Reviews Claude setup artifacts (preferences, project instructions, CLAUDE.md, rules, skills, agents, prompts) against a rubric and proposes edits. Use for "review my instructions", "review this skill", "is my setup ready for this task", or after a model change.
---

# Harness review

Judges the harness, meaning everything around Claude that shapes its behavior, on its merits. No session transcript is needed; session-retro covers learning from what happened in one. The standard: does each line change Claude's behavior the way the user intends, at a cost worth paying?

## Core rules

- **Propose, don't apply.** Present changes for the user to accept, unless they ask you to apply them.
- **Small, independent changes.** Number each change so the user can judge and accept it on its own. Assemble a full revised text only when asked, or when the source is under about 300 words. Label it a draft for the user to edit into their own words, since hand-written instructions tend to outperform generated ones.
- **Cut before adding.** Keep the user's wording where it works. Change how values and goals are expressed, never the values or goals themselves.
- **Quote what you criticize,** except sensitive personal details (health, identity, relationships). Refer to those lines by topic instead.
- **Stay proportionate.** List only what needs action, plus a one-line count of the rest.
- **Size verification to the stakes.** Default to cut-and-watch. Recommend an A/B test only for lines that are both uncertain and consequential (see Verification).

## Modes

Pick the mode from the request. If it's unclear, ask one question.

1. **Instructions:** preferences, project instructions, CLAUDE.md, rules. Apply the Rubric and the Carve-out check.
2. **Skill or agent:** a SKILL.md or agent file. Apply the Rubric and the Skill checks.
3. **Prompt:** a prompt the user is about to send, or a saved template. Apply the Prompt checks.
4. **Pre-flight:** the user describes a task they're about to start. Apply the Pre-flight checks.

Add a **model-change pass** to modes 1 and 2 when the user says the model changed, or the material predates the current model. Ask of each line "would the current model do this anyway?" and mark likely-unneeded lines for deletion.

## Rubric

Apply to every line. Each criterion gives the test, then the fix.

- **Behavioral:** names something Claude would visibly do? -> Rewrite as an action, or cut.
- **Triggered:** says when it applies, rather than "on complex topics"? -> Name the situation.
- **Non-default:** would Claude do it anyway? -> Cut.
- **Reasoned:** gives a short why where edge cases matter? -> Add "because...".
- **Consistent:** agrees with other lines and layers? -> Keep one source of truth.
- **Testable:** could a transcript show it was followed? -> Make the behavior observable.
- **Unambiguous:** has only one reasonable reading? -> Split into explicit sentences.
- **Positive:** says what to do, not only what to avoid? -> Pair each "don't" with a "do".
- **Right layer:** loaded exactly where it's needed? -> Move it (see Layers).

## Layers and carve-outs

- **Account preferences** (about 300 words): what applies to nearly every response. How to work with the user, a priority order, one-line patterns.
- **Memory:** facts about the user, not instructions.
- **Project instructions** (about 300 words, plus files): one domain's stance, method and done criteria.
- **Knowledge files:** facts, plans, objectives, decisions and domain references. They go in a repo's docs/knowledge/ folder or a project's knowledge files, routed by an index with one line per file (what it covers, when to read it). Always-loaded layers point to the index; they don't import it.
- **Skill:** content that is situational, procedural (a method, rubric, checklist or reference) and reusable. The description is always loaded; the body loads on use.
- **User CLAUDE.md, ~/.claude/rules:** engineering values and workflow across repos.
- **Repo CLAUDE.md, AGENTS.md, .claude/rules** (under 200 lines each; rules can be path-scoped): commands, architecture, conventions.
- **Hook,** with a test written first: rules that must hold every time.
- **Subagent:** work that needs isolation or tool limits.

**Carve-out check:** for each chunk of related lines in an always-loaded layer, ask whether it fits the Skill or Knowledge files row better. Keep the two kinds apart: knowledge (what's true) belongs in indexed files, and methods (how to do something) belong in skills. Apply the trigger test: if you can't write a one-sentence description of when to use it, it isn't a skill yet. Otherwise, mark it "carve out into skill <name>" and draft that description. Across skills, propose splitting one with two unrelated triggers, merging two whose triggers overlap, and retiring any that are unused or generic.

## Skill checks

- The description puts the key use case first, in words the user would type. Give 3 should-trigger and 3 shouldn't-trigger prompts.
- The most important instructions sit near the top. SKILL.md stays under about 500 lines, with detail in reference files one level down.
- Side-effect workflows set disable-model-invocation: true.
- Skills meant for claude.ai use only the frontmatter fields it accepts: name, description, license, compatibility, metadata, allowed-tools.
- Treat third-party skills as unreviewed unless the user says they've read them. Flag conflicts with other skills, and generic content a current model doesn't need.

**Method skills** encode how to run an interaction or produce a piece of work, such as a coaching protocol or a story-writing process. Also check that they:

- Separate the parts: guardrails (safety and when to stop), process (ordered steps Claude can copy and track), framework (the mental model or taxonomy), criteria (how to judge the result), and worked examples.
- Put guardrails first, especially for coaching or therapeutic methods, and defer to Claude's own safeguards.
- Match strictness to the stakes: loose guidance for conversations, a template for documents, a checklist or script for validation.
- Anchor criteria with observable levels and an example, not adjectives.
- Encode the user's choices, order and boundaries rather than restating a textbook method Claude already knows.
- Keep criteria in their own reference file, so a session or a judging subagent can apply them without the rest of the skill.
- Come with 3 or more test scenarios, written before the method was.

## Prompt checks

- **Goal:** says what outcome, and for whom.
- **Done when:** gives the test the result must pass.
- **Constraints:** scope, what not to touch, format, time box.
- **Context:** names the files, knowledge or examples Claude needs, or where to find them.
- **Questions first:** for work that's expensive to redo, asks Claude to interview the user before starting.
- **Recurrence:** if the user has sent this kind of prompt three times, propose it as a skill instead.

## Pre-flight checks

Given the task and the setup loaded in this session:

1. **Applicable:** which instructions, skills, knowledge files and connectors bear on the task.
2. **Gaps:** missing knowledge, skills or access the task will need.
3. **Conflicts:** instructions that pull in different directions for this task.
4. **Brief:** a suggested per-task brief (goal, done when, constraints) with the gaps filled in.

Recommend harness changes only for gaps likely to recur. One-off context belongs in the brief.

## Review output

1. **Verdict:** one sentence on the biggest problem, or "ready" for a pre-flight with no gaps.
2. **Changes:** a numbered table. Columns: line (quoted, or topic if sensitive), criteria failed, action (rewrite / move to <layer> / carve out / cut), proposed text.
3. **Verification:** cut-and-watch or A/B for each change. For A/B, give 2 or 3 realistic prompts.
4. **Draft:** only when the Core rules allow one.

## Verification

- **Cut-and-watch** (the default for low-stakes changes): apply the change, then check the next few sessions or retros for regressions.
- **A/B test** (only for uncertain, consequential lines): in Claude Code, offer to run it yourself. Write two temporary subagents with omitClaudeMd: true, each given one version as its prompt. Run each test prompt 2 or 3 times per version, then have a third subagent compare the outputs blind against the intended behavior. Elsewhere, give the user the prompts to run in fresh chats. For skills, use skill-creator's comparison and trigger tests.
