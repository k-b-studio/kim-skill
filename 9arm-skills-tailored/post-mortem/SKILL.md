---
name: post-mortem
description: Write the canonical engineering record of a fixed bug — root cause, mechanism, fix, validation, and how it slipped through. Tailored for solo software/full-stack/ML engineering work with no formal issue tracker — defaults to a local markdown file instead of a JIRA comment, and treats "owner" as optional. Covers layer-specific mechanics (frontend/API/backend/DB/infra for web bugs; metrics/dataset/checkpoint for ML bugs). Use after a debug session lands a fix, before moving on. Trigger on /post-mortem, when the user says "write the post-mortem / postmortem / RCA / root cause analysis", "document this fix", "write up the root cause", or hands over a fixed-and-validated bug and asks for the writeup.
---

# Post-mortem

The canonical record of a bug fix. Written **after** debugging lands a real fix, **for** future-you (who will have forgotten the details in 6 months) and for anyone else who later reads the repo. Code identifiers are welcome — this is the artifact that lets you recover the mental model fast, without a ticket system to lean on.

For a shareable, less-technical version of the same content (e.g. a GitHub release note, a writeup for a collaborator or client, a status-page note for a user-facing bug), hand the finished post-mortem to `management-talk`. Post-mortem owns the engineering truth; management-talk reframes it for a non-code-reading audience.

## When to invoke

- "/post-mortem"
- "write the post-mortem / postmortem / RCA / root-cause analysis"
- "document this fix" / "write up the root cause"
- After a debug session has clearly landed a fix, proactively offer to draft one.

## When NOT to use

- **Bug not fixed yet, or fix not validated.** A post-mortem of a hypothesis is misleading. Refuse and say what's missing.
- **Trivial fix** (typo, obvious one-liner, a copy tweak). The commit message is the record. Don't manufacture ceremony.

## Required inputs — refuse to draft without these

Before writing a single line, confirm all four. If any are missing, list what's missing and stop:

- [ ] **Reliable repro** exists (not "happens sometimes" — a deterministic or high-rate-flake repro that can be run again).
- [ ] **Root cause is known** (the mechanism is identified, not a hypothesis).
- [ ] **Fix is identified** (commit / branch / diff pointer).
- [ ] **Fix is validated** (the original repro now passes; for ML, the relevant metric is back in range; for web, the failing request/flow now succeeds in the environment(s) it failed in).

These map directly to `debug-mantra` steps 1–4. If you came in via `debug-mantra`, the breadcrumb list from step 4 is your raw material — pull from it.

**No formal tracker means no ticket key is required.** Skip that field rather than inventing one. **Owner defaults to you** unless the user names someone else — don't ask "who owns this" as a blocking question, just confirm the default silently and move on.

## Structure

Use these blocks in this order. **Summary, Root cause, Fix, and Validation are mandatory.** The rest are conditional but usually present.

### 1. Summary _(mandatory)_
One paragraph. What broke, in plain terms. What fixed it, in one sentence. Commit/PR reference if there is one. A reader who stops here should have the right answer.

### 2. Symptom
What was actually observed. Test output, error message, log line, stack trace, HTTP status/response body, browser console error — or for ML, the metric that moved (loss spiked, accuracy dropped, output diverged, NaN appeared) with the actual before/after numbers.

### 3. Root cause _(mandatory)_
The actual bug mechanism. **Code identifiers welcome and expected** — function names, file paths, variable names, route/endpoint names, table/column names, commit SHAs of the offending change. **Name the layer(s) involved** for a full-stack bug — client, API, server logic, DB, infra/config — and if it crosses layers, walk the handoff between them explicitly (e.g. "the client sent X, the API expected Y"). For ML bugs, this includes data-pipeline stage, config value, or training-loop step where things went wrong. Walk the cause chain end-to-end. This is the most expensive section and the reason the post-mortem exists at all.

### 4. Why it produced the symptom
Link the root cause to the symptom, especially when they're far apart — a DB constraint violation that surfaces as a generic 500 in the UI three requests later, or a preprocessing bug that only shows up as a training-time NaN three epochs in. Walk the chain so a reader who only knows the symptom can connect it back to the cause without re-deriving it.

### 5. Fix _(mandatory)_
What changed and **why this change addresses the root cause** rather than hiding the symptom. Reference the commit/diff. If a previous fix attempt papered over the symptom, name it and explain what was wrong with it.

### 6. How it was found
Short. The debugging path:
- What repro made it deterministic.
- What tools cracked it (debugger, browser DevTools/network tab, source tracing, knob enumeration, instrumentation — the `debug-mantra` step 2 cascade; note explicitly if no debugger was available).
- Which layer(s) it took to narrow down to, for a full-stack bug.
- Hypotheses tried and rejected, one-line reason each. (Pull from the breadcrumb list.)
- The single experiment that confirmed the cause.

### 7. Why it slipped through
What allowed this bug to exist unnoticed until now. Pick the real reason:
- No test/eval exercised this path or config.
- Latent code (correct when written, broken by a later change elsewhere).
- Workload/data gap (no real input reached this path until now — common in ML when a new dataset or edge case surfaces it).
- Incomplete prior fix (defensive check hid the symptom; root cause untouched).
- **Environment drift** (worked in dev, broke in staging/prod due to config, env var, or infra difference not mirrored locally).
- **Cross-layer contract mismatch** (frontend and backend deployed out of sync, or an API shape changed without the other side being updated).

If the honest answer is "no good reason — should have caught this," say so. **Blameless** — describe the gap, not yourself as if it were a character flaw.

### 8. Validation _(mandatory)_
How you know the fix works. Concrete:
- Original failing test/repro now passes.
- For web bugs: which environment(s) you validated in (local, staging, prod) and which browser(s) if relevant — don't imply prod-validated if you only checked locally.
- For ML: the metric that regressed is now back in range — give the actual before/after numbers (loss, accuracy, eval score), not just "looks better."
- If you only validated one configuration/dataset/environment/browser, say so explicitly. Don't imply broader coverage than you actually have.

### 9. Follow-ups _(optional)_
Concrete next-steps that aren't in the fix itself: a regression test to add, a related edge case to check later, a refactor worth doing, a staging/prod parity gap worth closing. If there are none, write *"None — the fix is sufficient."* Don't manufacture follow-ups to look thorough.

## Tone

- **Code identifiers are first-class.** Function names, file paths, variable names, route/endpoint names, commit SHAs, config keys — keep them. The whole point is that you can grep your way back to the change.
- **Mechanism over narrative.** Walk the actual cause chain. Don't soften it into vague language — say which function, which endpoint, which config value, which pipeline stage.
- **Active voice, concrete subjects, short paragraphs.**
- **No hedging.** "I believe" / "appears to" / "may have" — drop. State it or don't write it.
- **Blameless.** Describe the bug, the gap, and the fix.
- **No advocacy.** A post-mortem records what happened and what's next. A separate proposal is a separate proposal.

## Output flow

1. **Confirm all four required inputs are satisfied.** If any are missing, list them and stop. Do not draft.
2. **Confirm where it goes** (default: a local markdown file, e.g. `docs/postmortems/<short-slug>-<date>.md` in the project repo, or a `NOTES.md` entry — whichever the project already uses, or the simplest option if neither exists). No JIRA/ticket flow — write and save directly, no separate posting/sign-off step needed.
3. **Produce the draft**, then save it to the agreed location.
4. **One iteration is normal, three is a smell.** If you're still revising the third time, ask what specific section is wrong rather than keep tweaking blindly.
5. **Offer the management-talk handoff** only if there's an actual audience for it (a collaborator, a client, end users, a public writeup). Don't offer it by default for a solo internal fix.

## Worked example A — cross-layer bug (local project, no ticket)

> **Summary.** Checkout form silently failed to submit on the third retry for users with a saved payment method. Caused by the frontend sending a stale idempotency key after a client-side retry, which the API correctly rejected as a duplicate — but the frontend swallowed the 409 instead of surfacing it. Fixed on both sides: frontend now generates a fresh key per user-initiated submit, and the 409 case now shows an error instead of failing silently. Commit `f3a9c1e`.
>
> **Symptom.** Users occasionally clicked "Pay" and nothing happened — no error, no redirect, button just stopped spinning. Only reproduced after a slow network forced an automatic client-side retry.
>
> **Root cause.** Client layer: `checkout.ts::submitPayment` retries on timeout using the same `idempotencyKey` as the original request (by design, for safety). Server layer: `POST /api/checkout` correctly returns `409 Conflict` for a duplicate idempotency key, per spec. The bug is entirely in the client's response handling: the fetch wrapper in `api/client.ts` only treated `2xx` as success and `5xx` as retryable; a `409` fell through neither branch and was silently discarded.
>
> **Why it produced the symptom.** The retry logic and the idempotency logic were each correct in isolation — the gap was at the handoff between them: nothing in the client anticipated a *successful* retry protection producing a response it didn't know how to handle.
>
> **Fix.** Commit `f3a9c1e`. Client: explicit handling for `409` on the checkout endpoint — surfaces "Payment already processing, please wait" instead of dropping the response. Also stopped reusing the idempotency key across a user-initiated retry after the first automatic one, so genuine double-submits still get the safety net without silently eating a legitimate retry.
>
> **How it was found.** Repro required throttling the network in browser DevTools to force the timeout-retry path; console showed no error since the response was swallowed. Confirmed via the network tab that the second request returned `409` with the same key as the first — that pointed straight at `api/client.ts`'s response switch. No backend change was needed once that was clear.
>
> **Why it slipped through.** Cross-layer contract mismatch: the idempotency key behavior was implemented on the backend for a different reason (payment safety) and the frontend's retry logic was written without checking what response codes the endpoint could actually return. No test exercised the retry-after-timeout path end-to-end.
>
> **Validation.** Reproduced and confirmed fixed in local dev (throttled network) and in staging against the real payment endpoint. Not yet validated against the sandbox payment provider in prod config — planned before next deploy.
>
> **Follow-ups.** Add an integration test for the retry-then-409 path. (No owner needed — solo project — but flagged so it isn't forgotten.)

## Worked example B — ML bug (local project, no ticket)

> **Summary.** Training loss went to NaN at epoch 3 on every run of the fine-tuning script. Caused by an unclamped log() call in the custom loss function receiving a zero probability after softmax underflow in fp16. Fixed by switching the loss computation to fp32 and adding an epsilon clamp. Commit `a1b2c3d`.
>
> **Symptom.** `train.py` loss goes to `nan` at step ~1800 (epoch 3) on every run, regardless of seed. Eval metrics before that point looked normal.
>
> **Root cause.** `losses.py::weighted_ce_loss` computes `-log(p)` directly on the softmax output under fp16 autocast. Once a class probability underflowed to exactly `0.0` in fp16 (which starts happening reliably once the model gets confident enough, around epoch 3), `log(0)` produced `-inf`, and the next backward pass turned that into `nan` gradients that then poisoned every subsequent step.
>
> **Why it produced the symptom.** The underflow itself is silent — no error, no warning, just a `0.0` where a small positive number should be. It only becomes visible one operation later, at the `log()` call.
>
> **Fix.** Commit `a1b2c3d`: compute the loss in fp32 and add a `1e-8` epsilon clamp on the probability before `log()`.
>
> **How it was found.** Repro was deterministic, so went straight to instrumentation (remote GPU box, no debug port). `[DBG-91f2]` prints of the min non-zero probability per batch confirmed it hit exactly `0.0` at step 1798, two steps before the NaN. Confirmed fp16 was the cause by rerunning one epoch in fp32 only — no NaN.
>
> **Why it slipped through.** No eval covered probability values near the underflow boundary; the loss function was copied from an earlier fp32-only project and never audited for fp16 safety.
>
> **Validation.** Reran the full 5-epoch fine-tune 3 times with different seeds — no NaN in any run. Final eval accuracy matches the last known-good fp32 baseline (91.2% vs 91.4%, within noise). Not retested on the larger dataset variant.
>
> **Follow-ups.** None — the fix is sufficient. Worth auditing other custom loss functions for the same pattern, but that's a separate task.

## Rules

- **Refuse to draft without all four required inputs.**
- **Never invent root cause, validation numbers, or follow-ups.** If a section's facts aren't there, ask.
- **Never strip code identifiers.** They're the index back into the codebase.
- **Blameless.** Describe gaps and bugs, never yourself as if it's a character flaw.
- **State validation coverage honestly.** If you only tested one config/environment/browser, say so.
- **No ticket-system ceremony.** No sign-off step, no ADF payload, no "post it" — just write the file.
