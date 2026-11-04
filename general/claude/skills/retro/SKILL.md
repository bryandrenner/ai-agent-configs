---
name: retro
disable-model-invocation: true
description: Retro this session — diagnose friction, map each to the smallest Claude Code change, and draft the changes
argument-hint: "[optional focus, e.g. permissions, context use]"
---

# Session retro

Retro this working session. Focus area, if any: $ARGUMENTS

## Ground rules

- **Evidence or silence.** Ground every point in a specific moment from this session: quote it or point to the exchange. Generic advice is worse than nothing.
- **Critical, not complimentary.** Skip praise; I know where the session went fine.
- **Show inferences.** Where you inferred my intent, say what you inferred and why.
- **Draft, don't apply.** Don't edit any file. Draft every change; I'll decide what to apply.
- **Flag lossy evidence.** If parts of this session were compacted, say which and mark points drawn from them as low-confidence.
- **Scale to the session.** A short or smooth session earns a short retro. If context is nearly full, say so and deliver the top 3 only.

## Step 1 — Gather facts

Read the actual configuration before diagnosing; don't reason from assumptions about what's set up.

- **Instruction layers:** `~/.claude/CLAUDE.md`, `~/.claude/rules/`, every `CLAUDE.md` from the repo root down to the directories touched this session, `CLAUDE.local.md`, `.claude/rules/` (note each file's `paths:` scope), and auto memory (`MEMORY.md`).
- **Settings:** `~/.claude/settings.json`, `.claude/settings.json`, `.claude/settings.local.json` — permissions, default mode, hooks.
- **Extensions:** available skills, slash commands, subagents, MCP servers, and output style.
- **Feature currency:** check the installed version (`claude --version`). Before recommending a feature you're not certain exists in that version, confirm it with the `claude-code-guide` subagent or the official docs.

Note which loaded instructions actually shaped this session and which sat unused.

## Step 2 — Diagnose

Identify friction you actually observed, such as:

- **Intent gaps:** requests where you had to guess, or where I corrected you mid-stream.
- **Instruction-layer gaps:** context I repeated or explained that you had no way to know.
- **Flow breaks:** approvals or interruptions that cost momentum (permissions too tight, wrong mode).
- **Misplaced labor:** work I did manually that you could or should have done.
- **Miscalibrated trust:** checks of mine that added no value; errors of yours I didn't catch.
- **Your own errors:** mistakes, claims of "done" without verification, tests not run.
- **Wasted effort:** redundant reads or searches, dead ends, re-derived facts, overlong output, always-on instructions that never applied.
- **Model and effort fit:** work that would have gone better on a different model or thinking level, e.g. routine edits run at high cost, or design and debugging that stalled for lack of deeper reasoning.

Tag each friction **first seen** or **recurring**. Count it as recurring only with evidence: a correction this session that matches an existing instruction, an auto-memory note, or a repeat within this session. A first sighting usually earns a watch note, not a persistent change, unless the fix is trivial and low-risk.

## Step 3 — Missed opportunities

Scan your features for moments this session where one would have helped but wasn't used, whether I didn't invoke it or you didn't suggest it. For each: **moment → feature → what would have differed.**

Then list up to 3 capabilities you suspect I don't know about or underuse. Label these **general awareness**, since they aren't grounded in this session, and give one line each on what they're for.

## Step 4 — Map each friction to the smallest change

Levers to consider, without being limited to:

- **Workflow:** plan mode, permission modes, model and effort level (including per-subagent models), subagents, todos, checkpoints and rewind, compaction timing.
- **Instruction layers:** org policy · user (`~/.claude/CLAUDE.md`, `~/.claude/rules/`) · repo (`CLAUDE.md`, `AGENTS.md`) · subtree `CLAUDE.md` · path-scoped `.claude/rules/` · local overlay (`CLAUDE.local.md`) · auto memory.
- **On-demand behavior:** skills (loaded by description) and slash commands.
- **Enforcement and integration:** hooks, MCP servers, output styles, settings.

Climb this ladder only as far as the problem requires:

1. No persistent change — a habit adjustment for me or you
2. Trim or correct an existing instruction
3. Path-scoped rule or subtree `CLAUDE.md`
4. Skill or slash command (loads on demand)
5. Hook or setting (deterministic enforcement)
6. New always-on instruction (last resort)

If an instruction already exists and was ignored, don't recommend restating it louder. Diagnose why it failed (wrong scope, buried, ambiguous, conflicting) and consider whether a hook should enforce it instead.

## Step 5 — Deliver

1. **Top 3 changes**, ranked by impact per effort. For each: evidence (specific moment) → diagnosis → recurrence tag → the drafted artifact with its file path. For an existing file, show a diff; for a new file, its full content.
2. **Missed opportunities:** the session-anchored entries from Step 3, then the general-awareness list, labeled.
3. **One thing I should stop doing.**
4. **Instruction-layer cleanup:** anything in my always-on layers this session showed is unused, conflicting, or misscoped, and anything from this conversation that belongs in a persistent layer.
5. **Watch list:** first-seen frictions that didn't earn a change yet, one line each.
