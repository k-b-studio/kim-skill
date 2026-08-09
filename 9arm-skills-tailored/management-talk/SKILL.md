---
name: management-talk
description: Rewrite engineer-to-engineer content for a non-code-reading audience and shape it for the channel it is going to — GitHub issue/release note, email, README/changelog entry, status-page note, or a status update to a client/collaborator. Tailored for solo software/full-stack/ML engineering work — "the audience" is whoever isn't reading the code (a client, an end user, a collaborator on a different layer of the stack, an open-source issue thread, a future reader of your changelog) rather than a corporate management chain. Trigger when asked to write/rewrite something "less technical", a changelog/release note, a client or collaborator update, a GitHub issue summary, a status-page note, or a public-facing writeup of engineering work.
---

# Management Talk (solo/indie edition)

Same translation rules as the team version, minus the org chart. "Leadership" here means **anyone who needs the outcome without the mechanism** — a client, an end user, a collaborator working on a different layer of the stack (they own the frontend, you fixed the backend, or vice versa), someone reading a GitHub issue six months from now, or a changelog reader who never saw the code. The channel decides the length, formatting, and how much structure stays on the page.

## When to invoke

- "write this up for [client / collaborator / the issue thread / users]"
- "make this less technical" / "less jargony"
- "write a changelog entry / release note" for a fix or feature
- "summarize this for the GitHub issue"
- "write a status-page update"
- "write an update for [non-technical person]" or for a collaborator on a layer you didn't touch

If the audience is unclear, ask one short question — *"who's reading this — a client, a collaborator, end users, or is this going in a changelog?"* — and stop.

## Audience — what "non-code-reading" means here

Someone who cares about outcome, not mechanism: a client who wants to know if their thing works, an end user hitting a status page, a collaborator who owns a different part of the system (frontend vs backend vs infra) and doesn't need your internals, a future reader of a changelog/release note, or a GitHub issue thread where not everyone is deep in the code.

They want: *what changed, does it affect them, what do they need to do (if anything).* They do not want: function names, file paths, SQL, or the mechanism-level cause chain.

A **collaborator on a different layer** is a special case: they're technical, just not on this layer. Keep concept-level vocabulary for their own domain if relevant (a backend person reading about a frontend bug can handle "state management," "re-render," "hydration") but still strip the actual code identifiers from the layer they don't own.

This is **not** for a true ELI5/marketing audience (a landing page, a tweet) — that needs a different, punchier rewrite. Flag and confirm before producing one of those.

## Tone

**Keep.** Product/project name, feature name, version numbers, issue/PR numbers, dataset or model names, endpoint/page names if the audience already knows them by that name. These are the bridge between the engineering work and what the reader can reference later.

**Strip.** Function names, file paths, variable names, commit SHAs, code expressions, config keys, SQL, internal data-structure jargon.

**Translate.** Mechanism into one or two sentences of plain-English cause-and-effect. Not *"the loss function computed log(0) under fp16 autocast"* but *"training was unstable on longer runs due to a numerical precision issue — fixed by switching that calculation to higher precision."* Not *"a stale idempotency key caused the client to swallow a 409"* but *"a retry on a slow connection could silently cancel your payment instead of showing an error — fixed on both the checkout page and the server."* Translate without lying — a bug stays a bug; a regression stays a regression.

**Don't over-strip.** A reasonably technical audience (a client who commissioned software, a collaborator, an OSS issue thread) reads concept-level vocabulary fine — *bug, regression, race condition, precision, dependency, breaking change, workaround, caching, timeout*. The line is between *concept that matters here* (keep) and *the actual code identifier* (strip).

**Bias toward** active voice, concrete subjects, short paragraphs. *"Found the bug. Fixed it. Shipped in v0.4.2."* beats *"The root cause was identified and a fix was implemented and released."*

**Avoid:**
- Hedging that isn't really hedging (*"we believe," "may have"*). State it or don't.
- Restating the obvious for thoroughness.
- Engineering-process minutiae (bisect runs, debug iterations, which devtools tab). They care that it's fixed, not how — unless the process itself is the interesting part of the story (rare; call it out explicitly if so).

## Channel shapes

### GitHub issue / PR comment

Short, scannable, links inline.

- One bolded status line: *"Fixed in v0.4.2."* / *"Root cause found, fix in progress."* / *"Can't reproduce yet — need more info."*
- What was wrong, in plain terms (1–2 sentences).
- What changed / what to do (upgrade, no action needed, workaround).
- Link the commit/PR/release, not five links.

### Changelog / release note entry

One line, front-loaded with the verb and the user-facing effect.

- Pattern: *"Fixed: \<user-visible symptom\>."* / *"Fixed: checkout could silently fail on a slow connection retry."* / *"Fixed: training could produce NaN loss on runs longer than 3 epochs."*
- No mechanism unless the reader would care (e.g. a security fix might warrant one extra clause on impact).
- Group under Fixed / Added / Changed if the project already has that convention.

### Status-page / user-facing note

For an outage or visible bug users hit directly.

- **What happened** (plain terms, no blame, no internals): *"Some users were unable to complete checkout between [time] and [time]."*
- **Current status**: investigating / identified / fix in progress / resolved.
- **What to do**, if anything (retry, no action needed).
- Timestamps in the reader's likely timezone or explicitly marked UTC.
- No code, no root cause detail beyond what a user needs to trust it's handled.

### Client / collaborator update (email or message)

- **Subject/first line:** the outcome, as a phrase. *"Checkout bug affecting slow connections — fixed."*
- **Body:** 2–3 short paragraphs. What was wrong (in their terms), what you did, what's next if anything.
- Sign off with the next thing that needs their input, if any.

### Async status note (to a collaborator, standup-style)

- 1–2 lines. *"Fixed the checkout retry bug — was a client/server mismatch on duplicate-submit handling. Both sides updated."*
- No bullets, no bolded labels — the sentence is the format.

## Source material

The input is one of:

1. **A post-mortem you just wrote** (or the current conversation, if you just finished debugging) → reuse that as the source of truth. Don't re-derive facts.
2. **Pasted technical text** → use directly.

If the source is ambiguous, ask one question and stop.

## Output flow

1. **Confirm the channel** if not stated.
2. **Produce the draft** as a single block, formatted as the channel would render it.
3. **This skill only drafts — it doesn't post.** Hand the draft over; you decide where and when it goes out (GitHub, email, changelog file, status page). No sign-off ceremony needed since there's no separate posting step this skill performs.
4. **One iteration is normal, three is a smell.** If you're on the third revision, ask what specific framing is off.

## Rules

- **Never invent facts** to make the rewrite cleaner. If root cause is genuinely unknown, say so.
- **Never strip an issue/PR number or version number** — they're the cross-reference back to the real record.
- **Stay out of advocacy.** This produces a status update, not a pitch or a recommendation memo.
