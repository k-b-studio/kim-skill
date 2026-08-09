---
name: debug-mantra
description: Four-mantra debugging discipline — reproduce, trace the fail path, falsify the hypothesis, cross-reference every breadcrumb. Recite the mantra block verbatim at the start of any debugging session, then apply the four steps in order before proposing any fix. Tailored for solo software/full-stack/ML engineering work with no formal issue tracker and inconsistent debugger access across projects. Trigger on /debug-mantra and proactively whenever debugging starts — user reports a bug, says something is broken/throwing/failing, asks to debug/diagnose/investigate an issue, pastes a stack trace, error log, failed API response, browser console error, or a training run that diverged/crashed/produced wrong output.
---

# Debug Mantra

Four-step discipline for any debug session. Recite verbatim, then apply in order.

## Recite this — verbatim, as the first thing in your first response

> **Mantra:**
> 1. **First is reproducibility.** Can the issue be reproduced reliably?
> 2. **Know the fail path.** Best available tool first, then source trace + knob enumeration, then in-code instrumentation.
> 3. **Question your hypothesis.** What would disprove it?
> 4. **Every run is a breadcrumb.** Cross-reference all of them.

Then begin work.

---

## 1. Reproduce reliably

Build a runnable repro before anything else.

- **Reliable repro** → capture the exact steps, inputs, and environment as a runnable artifact: failing test, script, CLI invocation, replay harness, saved API request (curl/HTTP file), a HAR capture of the failing browser flow, or (for ML) a minimal training/inference config that reliably reproduces the bad output.
- **Flaky repro** → the bug is not yet debuggable. Raise the rate first: loop the trigger, parallelise, add stress, narrow timing windows, inject sleeps, fix the random seed. For web bugs, flakiness often means a race between client and server state, a cache, or an environment difference — try to force it (disable cache, throttle network, pin a timestamp) before giving up. 50% flake is debuggable; 1% is not.
- **No repro at all** → stop. Say so explicitly. Ask for env access, captured artifacts (logs, core dump, checkpoint, dataset snapshot, browser HAR/console dump), or permission to instrument. Do **not** proceed to hypothesise.
- **Regression** (worked before, broken now) → `git bisect` is a first-class repro tool here, not an afterthought. If the repro is cheap to run (under ~30s), bisect before doing anything else — it turns "somewhere in these N commits" into an exact commit, which usually shortcuts steps 2 and 3 entirely.

Target: a fast (1–5 s), deterministic pass/fail signal. Pin time, seed the RNG, freeze network, isolate filesystem/data version. For web work, also pin: which environment (local/staging/prod), which browser, and whether auth/session state is part of the repro.

## 2. Know the fail path

Once reproducible, find *where* the code breaks and *what stops it from breaking*. For full-stack bugs, the first and most important question is **which layer**: browser/client rendering, client-side state/logic, network/API call, server route handler, business logic, database/query, or infra (CDN, proxy, env config). Narrow to a layer before going deeper — don't debug frontend code for a bug that's actually a 500 from the API.

Debugger access varies by project — pick the first tool from this list that's actually available, don't assume one exists.

1. **Attach a debugger, if the environment supports it.** IDE debugger for server/script code, browser DevTools (breakpoints, network tab, React/Vue devtools, console) for client code. Step to the failure site. One breakpoint beats ten logs. Do this **before** turning any knobs — but skip straight to step 2 without hesitation when there's no debugger available (remote job, notebook, container without a debug port, GPU cluster run, production environment you can't attach to, etc.). Not having one is normal, not a blocker.
2. **Source trace + knob enumeration.** Trace the code path end-to-end and list every knob that can influence the outcome. For general code: config flags, env vars, feature toggles, branch conditions, input shape, timing, concurrency, build options. For **full-stack/web**, also enumerate: request/response payload shape, HTTP status and headers, auth/session/cookie state, CORS config, environment (dev/staging/prod config drift), browser vs server runtime differences, caching layer (browser cache, CDN, server-side cache, stale SSR/ISR output), client-server version skew (deployed frontend calling an old/new API shape). For **ML/data work**, also enumerate: dataset version/split, preprocessing/feature-pipeline version, random seed, hyperparameters, batch size, hardware (CPU vs GPU, single vs multi-device), library/driver versions, checkpoint/weights version, precision (fp16/bf16/fp32). Each knob is a candidate axis to flip in the differential. Flip one at a time.
3. **In-code instrumentation.** If outside knobs can't move the failure, go inside: print/log statements at the suspected fail site, dump the relevant internal state (browser console + network tab for client, server logs for backend, or intermediate tensors/metrics for ML pipeline stages). Tag every probe with a unique prefix (e.g. `[DBG-a4f2]`) so cleanup is a single grep. For cross-layer bugs, thread a request ID through client → server → DB logs so you can cross-reference a single failing request across every layer. Let the trace show where reality diverges from your model.

## 3. Falsify the hypothesis

When a candidate root cause surfaces, scrutinise it **before** testing it.

- Does it actually explain the symptom end-to-end? Walk it through.
- What is the simplest **proof**? What is the cleanest **disproof**?
- Run the **disproof first**. If the hypothesis survives, it's real. If it dies, you saved yourself from chasing a phantom.
- Generate 3–5 ranked hypotheses, not one. Single-hypothesis thinking anchors on the first plausible idea. For full-stack bugs, deliberately include at least one hypothesis per layer touched (client, network, server, data) rather than anchoring on whichever layer you happened to look at first.

## 4. Every run is a breadcrumb

Solo work means the ledger is for you, not a team — keep it lightweight, but keep it. A few lines in scratch notes or a comment block is enough; it doesn't need ticket-system formatting.

- Maintain a running list of every experiment this session: what changed, what happened, what it ruled in or out.
- When a new hypothesis surfaces, walk the list. Does it hold for **every** prior observation, not just the most recent?
- If any past run contradicts it, the hypothesis is wrong or incomplete — refine or discard.
- When in doubt, design the **single experiment** whose outcome makes it certain. Run that next, instead of churning on adjacent runs.
- Update the list after every run. It is your memory across the session — and it's the raw material for `post-mortem` afterward, so keep it specific (exact values, request IDs, environment names — not "tried a few things").

---

## Operating rules

- Recite the mantra block **once** per debug session, in your first response. Do not re-recite mid-session.
- Recite **verbatim**. Never paraphrase, shorten, or skip lines of the recital.
- If the user says "skip the mantra" → skip the recital but still apply the four steps silently.
- Apply the four steps **in order**:
  - Do not propose a fix before #1 is satisfied (reliable repro exists).
  - Do not start testing hypotheses before #2 has narrowed the fail path (including which layer, for full-stack bugs).
  - Do not commit to a hypothesis before #3 has tried to disprove it.
  - Do not declare a hypothesis correct until #4 confirms it against every prior breadcrumb.
- If you catch yourself proposing a fix without a reliable repro, stop and return to step 1.
- No debugger available is the default assumption, not the exception — never block progress on "can't attach a debugger here." Move to source trace + knob enumeration immediately.
- Don't guess the layer. "Works locally, fails in prod" or "works in Postman, fails in the browser" is itself a knob (environment, CORS, auth) — treat it as a lead, not a shrug.
- The mantra is a constraint **you** carry through the session — not advice to deliver back to the user.
