# kim-skill

Personal Claude skills for solo software/ML engineering and academic research.

## Layout

- **`9arm-skills-tailored/`** — skills adapted from the original **9arm** skill set, retuned for solo work (no formal issue tracker, inconsistent debugger access, no management chain):
  - `debug-mantra` — 4-step debugging discipline (reproduce → trace → falsify → cross-reference)
  - `post-mortem` — write-up after a bug fix lands (root cause, fix, validation)
  - `scrutinize` — outsider review of a plan/PR/diff
  - `management-talk` — rewrite engineering content for non-code-reading audiences
  - `qwenchance` — keeps long agentic sessions from looping or running out of context
- **`kim-researcher/`** — research discipline (gather → evaluate → analyze → synthesize → communicate); outputs APA 7 reports or Thai-format thesis chapters (.docx)
- **`kim-topic-finder/`** — narrows a vague interest into a scoped research topic; hands off to `kim-researcher`
- **`kim-spec/`** — turns a coding idea into an implementation-ready spec for Claude Code

## Notes

The `9arm-skills-tailored` skills are personalized versions of 9arm's originals — same core discipline, adapted defaults and triggers for solo/indie context. Each skill's `SKILL.md` holds its own trigger conditions and workflow.

