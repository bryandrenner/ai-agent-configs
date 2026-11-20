---
name: session-retro
description: Retros a session from the user's feedback and the loaded context, tracing each correction to the earliest instruction, skill or prompt change that would have helped. Use for "retro this session", "how could that have gone better", or "retro my chat about X".
---

# Session retro

Answers one question: how could this session have gone better? Mines the user's feedback in the session and traces each item to the earliest change that would have produced the result they preferred. Judging setup artifacts on their own, without a session, is harness-review's job.

## Core rules

- **Propose, don't apply.** Present changes for the user to accept, unless they ask you to apply them.
- **Evidence first.** Every proposed change cites the ledger entries or audit rows it addresses.
- **Cut before adding.** Prefer removing or tightening a line to adding a new one.
- **Quote the user's feedback,** except sensitive personal details (health, identity, relationships). Refer to those by topic instead.
- **Stay proportionate.** Quick by default; one or two changes, never a rewrite.

## Where to run it

- If the session is ending, run inline.
- In Claude Code mid-session, run in a fork, so it sees the conversation and its output stays out of the main thread.
- For high-stakes sessions, suggest a fresh subagent that reads the transcript under ~/.claude/projects/. It's more independent, but costs more tokens.
- For a past claude.ai chat, find it with past-chat search. Its loaded context may differ from today's, so mark the audit approximate.

**Depth:** quick by default. Run the full retro when the user asks, monthly, or when the session felt long, bloated or off course.

## Steps

1. **Ledger.** Scan every user turn, including messages sent mid-task. Record corrections, redirects, rejections, re-asks, requirements added late, stated preferences about format, depth or tone, and approvals. For each, note the turn, the quote, what Claude had just done, and the result the user preferred.
2. **Fix.** Classify each entry:
   - **Instruction:** the text, plus its layer.
   - **Skill:** a new skill or an edit, with the text.
   - **Prompt:** what the user could have said up front (goal, done when, constraints, an example), phrased as a reusable pattern.
   - **No change:** a one-off or a model miss. Say so rather than inventing a rule.
3. **Vet.** Check each instruction or skill candidate against harness-review's rubric and layer map; invoke that skill if it's available. If it isn't, at minimum confirm the candidate names a visible behavior, says when it applies, isn't something Claude does by default, and sits in the narrowest layer that loads when needed.
4. **Context audit** (full retro only, or when there were no ledger entries). List the user's own configuration that loaded: preferences, project instructions and files, CLAUDE.md and rules, memory files read, skills invoked, and connectors the user added. Exclude platform content. Mark each chunk:
   - **Used:** name the turn it shaped.
   - **Should have applied:** it loaded but wasn't followed, which usually points to a trigger or wording problem.
   - **Irrelevant:** to this kind of session (move it to a narrower layer), or just this once (leave it).
   - **Harmful:** it pulled a response off course.

   Estimate the size in words of the largest irrelevant chunks. In Claude Code, point to /context for real numbers. Your sense of what influenced you is approximate, so mark low-confidence calls.

## Quick output (under one screen)

- The three most significant ledger entries, with their fixes.
- One change (text and layer) and one line to cut.
- One thing that worked.

## Full output

1. **Ledger table:** #, turn, feedback, what Claude did, preferred result, fix type.
2. **Context audit table:** chunk, layer, mark, size, action. Only rows that need action, plus a count of the rest.
3. **Top changes:** one or two, each naming the ledger entries or audit rows it addresses.
4. **Prompt patterns:** up to three.
5. **One out:** a line to cut or tighten.
6. **Keep:** what worked, so later edits don't lose it.
7. **Rule of Three:** a fix seen in three retros becomes a skill, hook or agent.

If there were no ledger entries, run the context audit and stop.

## Verifying changes

Default to cut-and-watch: apply the change, then check the next few sessions or retros for regressions. For changes that are both uncertain and consequential, recommend an A/B test as harness-review describes.
