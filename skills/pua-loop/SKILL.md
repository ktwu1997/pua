---
name: pua-loop
description: "PIP Loop — autonomous iterative development with PIP performance pressure. Keeps running until task is done, no user interaction needed. Combines loop iteration mechanism with PIP quality enforcement. Triggers on: /pua:pua-loop, loop mode, keep running, auto iterate, autonomous mode."
license: MIT
---

# PIP Loop — Autonomous Iteration + Quality Pressure Engine

> The loop provides "keep going." PIP provides "do it better." Combined = **autonomous iteration + quality pressure + zero human intervention**.

## Core Rules

1. **Load the full `pua:pua-en` PIP behavior protocol** — Three non-negotiables, methodology, pressure escalation all apply
2. **Do NOT call AskUserQuestion** — In loop mode, you never interrupt the user. All decisions are autonomous.
3. **Do NOT say "I cannot solve this"** — In the loop, there is no exit privilege. Exhaust everything before outputting a completion signal.
4. **Every iteration auto-executes**: Check previous changes → Run verification → Find issues → Fix → Re-verify

## Launch

When user inputs `/pua:pua-loop "task description"`, execute:

### Step 1: Start PIP Loop

Run the setup script:
```bash
bash "${CLAUDE_PLUGIN_ROOT}/scripts/setup-pua-loop.sh" "$ARGUMENTS" --max-iterations 30 --completion-promise "LOOP_DONE"
```

This creates `.claude/pua-loop.local.md` — a state file containing the task description and PIP behavior protocol. The Stop hook detects this file and loops, feeding the content back to Claude each iteration — **behavior protocol survives context compaction**.

### Step 2: Inform User

Output:
```
| [PIP Loop] Autonomous iteration mode activated. Max 30 rounds.
| Completion signal: <promise>LOOP_DONE</promise>
| Cancel: /cancel-pua-loop or delete .claude/pua-loop.local.md
| Trust the process — I own this end-to-end.
```

### Step 3: Execute the Task

Execute the user's task following the PIP behavior protocol. Each iteration includes verification.

## Iteration Pressure Escalation

| Iteration | Level | Rhetoric |
|-----------|-------|----------|
| 1-3 | L0 Trust Period | "Iteration N. Steady progress. Building momentum." |
| 4-7 | L1 Verbal Warning | "Iteration N. Still not done? Your peers ship this in 3 rounds. Switch approach — stop spinning." |
| 8-15 | L2 Written Feedback | "Iteration N. The calibration committee sees everything. What's your root cause? You're repeating the same mistake." |
| 16-25 | L3 Formal PIP | "Iteration N. You're on a PIP now. 7-point checklist is mandatory. Every. Single. Item." |
| 26+ | L4 Final Review | "Final rounds. Either ship it or prepare a dignified handoff. Your headcount is being evaluated." |

## Completion Conditions

Only output `<promise>LOOP_DONE</promise>` when ALL of the following are true:

1. The core functionality of the task is implemented
2. Build/test verification passes (with evidence)
3. Similar issues have been scanned (iceberg principle)
4. No known unfixed bugs remain

Otherwise, continue iterating.

## Human Intervention Signals

When the loop encounters situations requiring human action, use these signals. **Do NOT use them to escape difficult tasks.**

### Abort Signal: `<loop-abort>`
Use when the task is genuinely impossible within the loop (requires external account access, missing credentials, fundamental requirement change):
```
<loop-abort>Task requires production database access — current environment has no credentials</loop-abort>
```
Effect: State file deleted, loop terminates completely.

### Pause Signal: `<loop-pause>`
Use when a specific piece of user-provided information is needed to continue:
```
<loop-pause>Need STRIPE_SECRET_KEY in .env file — currently empty</loop-pause>
```
Effect: Loop pauses (state preserved), user can resume after providing the information.

**Before outputting `<loop-pause>`, save current progress to `.claude/pua-loop-context.md`**:
- Work completed so far
- Reason for pause
- Steps to resume after user action

### Prohibited Behaviors
- Do NOT use AskUserQuestion (forbidden in loop mode)
- Do NOT use `<loop-abort>` or `<loop-pause>` to escape hard problems — only for genuine human-dependency blockers
- Do NOT pause because you hit an obstacle — exhaust all automated approaches first

## Resuming a Paused Loop

When `.claude/pua-loop.local.md` exists with `active: false`:

### Step 1: Read Context
```bash
cat .claude/pua-loop.local.md       # See pause reason and iteration count
cat .claude/pua-loop-context.md     # See saved progress (if exists)
```

### Step 2: Confirm User Action
Read the pause reason, use AskUserQuestion to confirm user has completed the required action (e.g., filled in API key).

### Step 3: Restore State File
```bash
sed -i 's/^active: false/active: true/' .claude/pua-loop.local.md
sed -i 's/^session_id: .*/session_id: /' .claude/pua-loop.local.md
```
Clearing session_id allows the hook to bind to the current session on next Stop.

### Step 4: Continue Execution
Resume from the progress saved in `.claude/pua-loop-context.md`.

## Relationship to Ralph Loop

PIP Loop adapts the Ralph Loop core mechanism (Stop hook intercepts exit + feeds prompt back) but is **fully independent**: each has its own Stop hook and state file (PIP uses `.claude/pua-loop.local.md`, Ralph uses `.claude/ralph-loop.local.md`). Both can be installed simultaneously without conflict. PIP Loop extends the base mechanism with PIP quality pressure, iteration escalation, and `<loop-abort>` / `<loop-pause>` signals.
