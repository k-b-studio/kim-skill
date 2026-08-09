---
name: qwenchance
description: Keeps a long agentic task on-track — breaks out of looping/circular thinking, watches the context budget, bounds internal reasoning, and triggers a clean handoff before the window fills. Applies equally to long debugging sessions (full-stack web or otherwise) and long ML experimentation/hyperparameter-sweep sessions. Use when the model is repeating steps, re-reading the same files, restarting the dev server repeatedly, re-testing the same UI flow, re-running the same sweep configuration, second-guessing in circles, stuck or spinning, or running a long multi-step task at risk of exhausting context. Also use when the user says it is "looping", "going in circles", "stuck", "repeating itself", or asks for a handoff before running out of context.
---

# Staying on Track

Long, multi-step work fails three ways: **looping**, **over-thinking**, and **running out of context**. Run the checklist below **before each step**. When a trigger fires, do the matching action — don't deliberate about it.

## Before each step — run this

| Check | Trigger fires when... | Do this |
|---|---|---|
| **Looping?** | You're about to repeat an action (see signals below) | Break the loop — pick one fix below |
| **Over-thinking?** | You've reasoned past ~1000 words without acting | Stop. Act on your current best decision, or ask one question |
| **Context tight?** | A low-context reminder appeared, **or** 2+ budget signals hold | Finish this step, then hand off |

If nothing fires, take the step.

## 1. Loops — detect and break

A step is a loop if **any** of these is true:

- You're re-reading a file you already read this session (and it has **not** changed since).
- You're re-running a command/tool with the same args, expecting the same result.
- You're returning to a hypothesis you already tried and dropped.
- You're "reconsidering from the start" with no new evidence.
- The last 2 steps gained no new information.
- **Web-specific:** you're restarting the dev server, clearing cache, or re-testing the same click-through flow for the third-plus time without a new variable changed since the last attempt. Also watch for bouncing between frontend and backend fixes without confirming which layer actually owns the bug first (see `debug-mantra` step 2) — that's often a sign the layer wasn't narrowed down before starting to edit.
- **ML-specific:** you're re-running the same hyperparameter config (or a trivially nudged variant) for the third-plus time without a hypothesis for why this run would differ from the last. Sweeping is not looping *if* each run tests a distinct, stated hypothesis — it is looping if it's really "let's just try again."

**Re-reading a file you just edited is NOT a loop** — that's verifying. **Re-running training with a deliberately changed variable is NOT a loop** — that's the sweep. **Restarting the dev server after an actual code change is NOT a loop** — that's the normal edit-reload cycle.

When a loop fires, **stop** and do exactly one:

1. State the blocker in one sentence and ask a specific question.
2. Write what you know vs. don't know, then take a **different** action than last time.
3. Looped 2+ times on the same sub-problem? Declare it unsolved-for-now; move on or hand off.

Never repeat a failed action hoping for a different result.

**Retry cap:** never run the same failing command a 3rd time. Can't get something working (a command, a test runner, an import, a build, a training run that keeps crashing the same way) after ~3 attempts — *even varied ones* — STOP and flag it; don't grind through more variations.

**Don't edit blind** — it's the top loop source. Read enough to know the change is correct *before* editing. After each edit, verify it (read the diff / run it / run the test / reload the page and check the actual behavior) **before** the next step. One edit → one check.

## 2. Thinking — keep it bounded

Cap reasoning at **~1000 words per step**.

- Decide → act → observe. Don't re-derive a decision you already made.
- Can't decide in ~1000 words? The task is underspecified — ask one sharp question.
- Don't restate the whole problem to yourself. Reference what you concluded; don't rebuild it.

## 3. Context budget — count signals, don't estimate

**Authoritative:** A system reminder about low context / approaching auto-compaction. → **Hand off now** (section 4). Don't start new work.

**Otherwise, count how many of these are true right now:**

- [ ] 20+ assistant turns into the task.
- [ ] Read 5+ files, or any one huge file/log/dump (including large training logs, metric dumps, or verbose build/server logs).
- [ ] Long tool outputs you keep scrolling back to.
- [ ] 3+ plan steps still left.

**Count the boxes that are true, then map the count to an action:**

- **Count is 0 or 1 → CONTINUE** working normally.
- **Count is 2, 3, or 4 → HAND OFF** — finish the current step, then go to section 4.

Count first, then decide — don't judge by feel. Being on the last step or "almost done" does **not** lower the count or cancel a HAND OFF.

Before any **expensive** step (large read, new subtask, long generation, a new sweep run, a full app rebuild), ask: *"Room to finish this AND hand off after?"* If the count says HAND OFF, finish the current atomic unit, then hand off — don't start the next.

## 4. Hand off cleanly

When context is tight or asked:

1. **Land durable artifacts first** — save the file, commit, write the result, checkpoint the model. Nothing lost.
2. **Write a short handoff note yourself** (no formal tracker to lean on): what's done, what's in flight, what's next, which layer/component you were working in, and — if this was a debug session — point to the breadcrumb list from `debug-mantra` so the next session doesn't re-derive it.
3. State plainly: "Context is getting tight — handing off now; start a fresh session."
