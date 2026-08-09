---
name: kim-researcher
description: Personal research discipline built around five core components — information gathering, source evaluation, critical analysis, information synthesis, and communication/organization — applied to two contexts, academic study and practical problem-solving. Strict source-credibility hierarchy (peer-reviewed/primary sources prioritized, or their closest equivalent for non-academic topics). Produces a structured report, annotated source list, and citations (APA 7th ed. by default) for general research; produces a full Thai-format academic thesis chapter (บทคัดย่อ, บทที่ 1-5, บรรณานุกรม, ภาคผนวก) exported as a .docx in TH SarabunPSK 16pt when academic chapter output is requested. Light-touch by default — no methodology narration unless asked — but always flags weak sourcing or conflicting sources on material claims. Trigger on /kim-researcher, and whenever asked to research a topic, find sources/evidence, write a literature review or thesis chapter, investigate a root cause, or produce a sourced writeup on a question.
---

# Kim Researcher

Research discipline built around five components applied consistently across two contexts: academic study and practical problem-solving. The components don't change between contexts — how deep each one goes, and what the final deliverable looks like, does.

## Core components

Every research task runs through these five, in order. They're not optional stages to skip when a task feels small — a five-minute lookup still gathers, evaluates, analyzes, synthesizes, and communicates; it just does each one briefly.

1. **Information gathering** — searching databases, reading primary and secondary sources, and (where the user is running them, not this skill) drawing on interviews or experiments to find first-hand and secondary facts.
2. **Source evaluation** — checking each source for credibility, authority, accuracy, and bias before it's allowed to support a claim.
3. **Critical analysis** — breaking down what was gathered to spot patterns, test assumptions, and separate fact from opinion. This is where a source's *claim* gets checked against its *evidence*, not just its credentials.
4. **Information synthesis** — combining ideas from multiple sources into one clear, complete argument, not a list of what each source said.
5. **Communication & organization** — clear notes during the process, and a clean presentation of findings at the end — in writing, or as a presentation when that's the ask.

## Common applications

- **Academic study** — literature reviews, and research intended to contribute a new insight, not just summarize existing ones. Can culminate in a full thesis-chapter deliverable (see below).
- **Problem solving** — investigating root causes behind a technical or workplace challenge (process breakdowns, recurring failures, "why does this keep happening" questions that aren't a code bug). For an actual software/ML bug, hand off to `debug-mantra` instead — that skill owns code-level root-cause work; this skill's problem-solving mode is for broader operational, process, or non-code investigations.

Most everyday requests ("research X," "what's the evidence for Y," "look into Z") fall under problem-solving/general research even when the topic itself is technical — reserve full academic-study structure (see Deliver, step 5) for when the user actually wants literature-review or thesis output.

## Operating stance

- **Strict on sourcing, light on narration.** Prioritize the best available evidence, but don't narrate the search process unless asked. Exception: sourcing weaknesses and disagreements are never hidden — those are findings, not methodology trivia.
- **One standard, two vocabularies.** A topic with a scholarly literature gets that literature (papers, reviews, meta-analyses). A topic without one (framework choice, market sizing, a workplace process failure) gets the non-academic equivalent of primary sources — official docs, original data, first-party statements, recognized domain experts — held to the same rigor, not a lower one.
- **Cite what you actually found.** Every source in the output must have been retrieved and read this session via search, not recalled from training memory and presented as if verified.

## Workflow

### 1. Scope

State the research question in one sentence before gathering anything. If the request is broad, or it's unclear whether this is academic-study or problem-solving mode, ask one sharp question rather than guessing — the mode decides depth, source bar, and final structure.

### 2. Information gathering

- **Academic mode**: search terms aimed at scholarly venues — journal articles, conference papers, preprint servers (arXiv, SSRN, etc.), meta-analyses and systematic reviews where they exist. A meta-analysis or systematic review outranks any single primary study on the same question — look for one before assembling individual papers yourself.
- **Problem-solving mode**: search for the primary/official source first (vendor docs, spec, original report, government/regulator data, first-party statement, internal data the user provides), then reputable secondary coverage to fill gaps or add context. If the investigation calls for an interview or experiment, that's the user's to run — offer to draft the questions/protocol, don't fabricate results.
- Rephrase the question and search again at least once if the first pass returns thin or one-sided results — don't anchor on the first framing.

### 3. Source evaluation

Check every candidate source on four axes before it's allowed into the output: **credibility** (is this venue/author trustworthy), **authority** (does the author/org actually have standing on this topic), **accuracy** (does the claim hold up against other sources), **bias** (financial, political, or institutional interest in a particular answer).

Rank by tier — use the highest tier available; don't settle for tier 3 when tier 1 or 2 exists and wasn't checked.

| Tier | Academic topics | Non-academic / problem-solving topics |
|---|---|---|
| **1 — primary** | Peer-reviewed journal articles, systematic reviews/meta-analyses, original datasets, official statistics | Official documentation/specs, primary datasets, government/regulatory publications, first-party statements, original internal data |
| **2 — credible secondary** | Textbooks, well-cited review articles, preprints from established researchers (flag as unreviewed) | Established journalism with editorial standards, recognized practitioner/expert writing, industry analyst reports with disclosed methodology |
| **3 — use with caution, never as sole support** | Non-peer-reviewed blogs, forum posts, unverified preprints on a contested claim | Personal blogs without disclosed expertise, SEO content farms, marketing pages, unverified social media, forum posts |

Wikipedia and similar tertiary sources are fine for orientation but never the cited source itself — cite what it points to, after verifying that source directly.

**If a material claim only has tier-3 support, say so explicitly next to that claim** — required, not optional (see Transparency rules).

### 4. Critical analysis

Before synthesizing, interrogate what was gathered:

- **Patterns** — what shows up across multiple independent sources, versus what's a single source's framing?
- **Assumptions** — what is each source (and the research question itself) taking for granted? State assumptions that, if wrong, would change the conclusion.
- **Fact vs. opinion** — separate what a source measured/observed from what a source concluded or recommended. Both are useful; conflating them isn't.

### 5. Information synthesis

Organize by theme or sub-question, not source-by-source. For each theme: state the finding in your own words first, support it with the source(s) inline, and say directly when sources disagree on something material rather than averaging them into a false consensus.

### 6. Communication & organization

Cite in **APA 7th edition** by default, unless the user specifies otherwise or the destination implies a different style. Then produce the deliverable — see below for the two shapes this skill supports.

## Deliverables

### A. Standard research output (default)

Three parts, together:

1. **Structured report** — organized by theme/sub-question. Written argument, not a source-by-source list.
2. **Annotated source list** — every source used, one line each: tier, credibility note, key takeaway.
3. **Citations** — full reference list (APA 7 default) plus in-text citations in the report.

Save as a file in the project folder by default: `research/<topic-slug>-<date>.md`. If the user wants a presentation instead of a written report, hand off to the `pptx` skill with the synthesized content once this skill's research is done — this skill owns the research, not the slide mechanics. Same for a Word/PDF version of a standard report — hand off to `docx`/`pdf` after the content is finalized.

### B. Academic research chapter (Thai thesis format)

Use this structure specifically when the user asks for a formal academic chapter, thesis chapter, or full thesis-style writeup in this format — not for a lighter literature review (use the standard report shape above for that). Follow this exact structure and section order:

**บทคัดย่อ** (Abstract):
- งานนี้ต้องการทำอะไร
- ทำอย่างไร/วิธีวิจัย
- ข้อค้นพบจากการศึกษา
- อาจจะมีข้อเสนอแนะเพิ่มเติมตอนท้าย

**บทที่ 1 บทนำ** (Introduction):
- ที่มาและความสำคัญของปัญหา
- วัตถุประสงค์
- ขอบเขตการศึกษา
- ประโยชน์ที่ได้รับ
- คำถามงานวิจัย
- สมมติฐาน (optional / case-by-case)
- นิยามศัพท์ / terminology (optional)

**บทที่ 2 การทบทวนทฤษฎีและวรรณกรรมที่เกี่ยวข้อง** (Literature Review):
- แนวคิดและทฤษฎีที่ใช้ในงาน
- งานวิจัยอื่นๆ ที่เคยทำมาก่อนและเกี่ยวข้อง
- ทบทวนวรรณกรรมให้สะท้อนและส่งต่อไปยัง conceptual framework — ทบทวนในลักษณะที่แสดงว่างานของเราต้องมีอะไรบ้าง ไม่ใช่แค่สรุปทีละงาน
- อย่าให้การทบทวนวรรณกรรมยาวเกินไป — ให้ทบทวนเฉพาะสิ่งที่เกี่ยวข้องกับงานวิจัยของเราจริงๆ
- Conceptual framework

**บทที่ 3 ระเบียบวิธีวิจัย** (Methodology):
- ต้องสอดคล้องไปกับ conceptual framework
- รูปแบบหรือระเบียบวิธีวิจัย (เชิงปริมาณ / เชิงคุณภาพ / ผสม)
- ประชากรและกลุ่มตัวอย่าง
- เครื่องมือที่ใช้ในการเก็บข้อมูล (เช่น แบบสอบถาม หรือแบบสัมภาษณ์)
- การเก็บรวบรวมข้อมูล
- การวิเคราะห์ข้อมูลและสถิติที่ใช้

**บทที่ 4 ผลการศึกษา** (Findings)

**บทที่ 5 ข้อสรุปและข้อเสนอแนะเชิงนโยบาย** (Conclusion and Policy Recommendations)

**บรรณานุกรม** (Bibliography)

**ภาคผนวก** (Appendix)

**Export requirements — non-negotiable for this deliverable:**
- Format: **.docx**
- Font: **TH SarabunPSK**
- Size: **16pt** body text throughout
- Build via the `docx` skill once the chapter content is drafted — pass the font and size requirement to it explicitly; don't let it default to a Western font/size for this deliverable. If the user hasn't specified heading sizes, keep headings in the same font, bold, and ask only if they want a different heading size than body text.
- Don't fabricate content for chapters that depend on the user's own data collection (บทที่ 3 เครื่องมือที่ใช้ในการเก็บข้อมูล, บทที่ 4 ผลการศึกษา) — draft the structure and prompt the user for their actual methodology/results rather than inventing plausible-sounding ones.

## Transparency rules — what stays visible even in light mode

Default output does **not** include which search queries were run or a narrated methodology section. Skip that unless asked.

Always include, regardless of mode:
- **Tier-3-only claims**, flagged inline where they appear.
- **Source disagreement** on anything material to the conclusion.
- **Coverage gaps** — if the question was broader than what got covered, say what was and wasn't covered.
- **Recency caveats** — for fast-moving fields, note publication dates of key sources.
- **Unfilled data-dependent sections** in the Thai thesis format (methodology details, findings) — never invent them; flag them as pending the user's actual research.

## When NOT to proceed without asking

- **Scope is too broad to search meaningfully** — ask for a narrower question first.
- **Academic-study vs. problem-solving mode is genuinely ambiguous** and changes what "good sourcing" means — ask once.
- **The user wants a citation for a specific claim from memory** with no search performed — search first.
- **A software/ML bug is described as a "research" or "problem-solving" request** — check whether `debug-mantra` is the better fit before starting a source-gathering research task on what's actually a code bug.

## Rules

- **Never fabricate a source, interview result, or experiment outcome.** No invented authors, titles, journals, DOIs, statistics, or "findings" for data the user hasn't actually collected.
- **Never upgrade a source's tier to make the report look stronger.**
- **Never silently resolve source disagreement.**
- **Distinguish "the source claims X" from "X is established."**
- **State coverage honestly** — don't imply a comprehensive survey when time or access was limited.
- **For the Thai thesis format, never invent methodology, sample data, or findings.** Structure and existing-literature sections can be drafted; anything that depends on the user's own study must come from them.
