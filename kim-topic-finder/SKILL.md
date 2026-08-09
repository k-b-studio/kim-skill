---
name: kim-topic-finder
description: Turns a blank page, an overly broad interest, or a roughly-worded idea into a shortlist of workable research topics — each with a properly scoped title, 2-3 research questions, and a gap statement grounded in an actual (light) literature scan, not invented. Covers all three common blockers — no ideas yet, topic too broad to narrow, or an idea that's hard to phrase/scope. Hands off to `kim-researcher` once a topic is picked (the shortlist output maps directly onto that skill's ที่มาและความสำคัญของปัญหา / วัตถุประสงค์ / คำถามงานวิจัย sections for the Thai thesis format). Trigger on /kim-topic-finder, and whenever the user says they don't know what to research, need a thesis/research topic, have an interest but can't narrow it down, want to check if a topic is too broad/already done, or need research questions for a topic.
---

# Kim Topic Finder

Solves the topic-formulation problem specifically — before there's a topic, there's no research to do. This skill's only job is to get from "I don't know what to study" (or "I have an idea but it's shapeless") to a short, real list of well-formed candidates. Once one is picked, `kim-researcher` takes over for the actual research and writing.

## The three starting points

Figure out which one applies before doing anything else — they need different handling:

1. **No ideas yet.** Blank page. Start from Interest elicitation (step 1).
2. **Too broad to narrow.** Has a field or general interest ("I want to study something about social media and mental health") but no specific angle. Start from the Narrowing funnel (step 3).
3. **Rough idea, hard to phrase/scope.** Has something close to a topic already but it's vague, unfocused, or not worded like a real research topic. Start from the Narrowing funnel (step 3), using their existing idea as the funnel's top level instead of eliciting one.

## Workflow

### 1. Interest elicitation (only for "no ideas yet")

Don't ask "what do you want to research" cold — that's the question they can't answer. Ask around it instead:

- Field/domain and academic level (undergrad, master's, PhD, or non-academic/professional research) — this sets how narrow "narrow enough" needs to be.
- What courses, readings, or projects actually held their attention, even briefly.
- A real problem they've personally run into, at work or otherwise, that nobody's fully explained.
- Whether there's an advisor, program, or client with a research area/mandate this needs to fit inside.
- Any hard constraints up front: timeframe, access to data/population, language, geographic scope.

Two or three of these usually surface a workable starting field. Move to step 2.

### 2. Landscape scan

Before generating candidates, do a **light** scan of the field — not the deep research `kim-researcher` would do, just enough to know what's active, what's contested, and where explicit gaps have already been named.

- Search recent review articles, meta-analyses, or survey papers in the field first — they're the fastest way to see the shape of what's known.
- Note any **explicit gap signals**: a paper's "future research" / "limitations" section naming what wasn't covered, a contradiction between studies that hasn't been resolved, a population/context/time period that keeps getting excluded.
- This is orientation, not proof — a handful of searches, not a systematic review. Say so if asked, but don't over-claim coverage in the output.

### 3. Narrowing funnel

Take the starting point (broad interest, or the user's rough idea) and narrow it through these levels. Not every level needs a hard answer immediately, but a workable topic needs most of them pinned down:

- **Field** → the broad area (e.g. "social media and mental health").
- **Population/context** → who or what specifically — age group, industry, organization type, country/region, platform.
- **Variable/relationship** → what specifically is being examined — not "the effect of X" in the abstract, but which two (or more) concrete things are being related, and how (correlation, causal, comparative, descriptive).
- **Time/scope boundary** → a period, a version, a specific policy window — something that stops the topic from silently expanding.
- **Angle** → what makes this specific narrowing worth doing instead of a nearby one — usually surfaces directly from a gap found in step 2.

A topic missing population/context or variable/relationship is still too broad — don't package it yet.

### 4. Generate candidates

Produce **3 candidates**, each arrived at through a different generation strategy so they're genuinely different options, not three phrasings of the same idea:

- **Gap-based** — directly targets an explicit gap or unresolved contradiction found in step 2. Usually the strongest candidate for novelty.
- **Context-extension** — takes an established finding/study and asks whether it holds in a population, region, industry, or time period it hasn't been tested in yet.
- **Problem-based** — starts from a real practical problem (from step 1, or found during the scan) and frames a topic that would help solve or explain it.

If the user's rough idea (starting point 3) is strong, keep it as one of the three and generate two alternatives around it rather than discarding it.

### 5. Package each candidate

For each of the 3, produce:

- **Topic title** — properly scoped and worded (population/context + variable/relationship + boundary, not a vague phrase). Match the phrasing convention of the field/level (e.g. Thai-thesis-style titles read differently than a business case-study title — ask if unsure which register).
- **2–3 research questions** — specific and answerable, not restatements of the title. These map directly into `kim-researcher`'s Thai-format คำถามงานวิจัย when that's the destination.
- **Gap statement** — one short paragraph: what's missing or unresolved in existing work, and why this topic addresses it. Must trace back to something actually found in step 2 (a named limitation, a contradiction, a context nobody's covered) — not an assertion that "not much research exists" without having checked.
- **Feasibility note** — one line: is data/access realistic for this population and timeframe, given what the user told you in step 1.

### 6. Hand off

Once the user picks one, the topic title feeds `kim-researcher` directly:
- Title → ที่มาและความสำคัญของปัญหา framing / research title.
- Research questions → คำถามงานวิจัย.
- Gap statement → the seed for บทที่ 2's literature review framing and the conceptual-framework direction.

Offer the handoff explicitly rather than starting the full research yourself — topic selection and full research are different jobs even though they're related.

## Rules

- **Never invent a gap.** A gap statement must be traceable to something actually found in the landscape scan (a stated limitation, a contradiction, an unexplored context) — not "there doesn't seem to be much research on this," said without having searched.
- **Never present three variations of the same idea as three candidates.** Each must come from a distinct generation strategy (step 4) so the choice is real.
- **Don't package an under-narrowed topic.** If population/context or variable/relationship is still missing after the funnel, that candidate isn't ready — narrow it further before including it in the shortlist.
- **State scan coverage honestly.** This is a light scan, not a systematic review — say so if the user is about to commit based on it, especially before claiming something is definitely novel.
- **No topic is "definitely novel."** The most honest claim is "not found in the sources checked" — say that, not "no one has done this."
