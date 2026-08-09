---
name: kim-spec
description: Turn a coding idea or feature request into an implementation-ready markdown spec file for Claude Code to execute in the terminal. Trigger whenever Kim describes something he wants built in code — a feature, script, refactor, integration, CLI tool, or app — even if he doesn't explicitly ask for a "spec" or "markdown file." Also trigger on /kim-spec. Do NOT use for research writeups (use kim-researcher) or writeups of bugs already fixed (use post-mortem) — this skill is forward-looking, for describing what should be built before it exists.
---

## Why this exists

Kim describes a coding idea in chat. This skill turns it into a markdown spec file, which Kim then hands off to Claude Code in a terminal to actually implement. The MD file is not a summary of the conversation — it is the ONLY context the future Claude Code instance will get. It must stand alone and be actionable without any other context from this conversation.

## Before writing

If the idea is genuinely underspecified — ambiguous scope, unclear tech stack, missing constraints — ask 1-2 clarifying questions first. But don't over-ask: bias toward drafting. Kim will iterate on the spec afterward, and Claude Code will surface further ambiguities during implementation anyway. When you notice a real ambiguity or a logical snag in the idea (e.g., a requirement that's internally inconsistent), it's usually better to resolve it with a stated assumption and flag it explicitly inside the spec as a decision point, rather than blocking the draft on it.

## Spec template

Use this template, adapting section depth to the size of the idea — a one-line idea gets short sections, a complex feature gets real depth. Bullet lists are fine here; this is a spec artifact, not conversational prose.

```
# [Feature/Idea Title]

## Goal
What this accomplishes and why it matters. 1-3 sentences.

## Requirements
Concrete, testable requirements.

## Proposed approach
Implementation strategy — architecture, key decisions, sequence of steps. Enough for Claude Code to start without re-deriving the design, but not so prescriptive it forecloses better approaches found during implementation. Call out any assumptions (tech stack, libraries, patterns) explicitly so Kim can override them.

## Files / modules affected
Best guess at what will need to change or get created. Mark speculative items as such (e.g. "may be replaced entirely by X").

## Acceptance criteria
How to know it's done — concrete checks Claude Code (or Kim) should run before calling it complete.

## Non-goals (optional)
Anything explicitly out of scope, if there's a real risk of scope creep.
```

## Save location

Save to `specs/` in the project root. If the project already has an established convention (e.g. `docs/specs/`), check for it first and follow it instead. File name is a kebab-case slug of the title, e.g. `specs/user-auth-refresh-tokens.md`. No dates in the filename unless Kim asks for one.

## After writing

Tell Kim the file path and give one line on what's in it. Don't paste the whole spec back into chat — Kim will open the file directly, and pasting it back is redundant with what he just watched get written.
