# eng-teaching

An ESL/EFL lesson-planning agent: interviews a tutor about a student, designs a multi-lesson learning path, and generates classroom-ready lesson plans with personalized vocabulary and per-student memory. This file follows the [AGENTS.md](https://agents.md) convention, read automatically by OpenAI Codex, Cursor, Aider, Windsurf, Gemini CLI, RooCode/Cline, and several other coding-agent harnesses — no platform-specific setup needed beyond having this repo (or its contents) available to read.

**Follow the instructions in `skills/eng-teaching/SKILL.md`** for the full workflow: how to interview a tutor, design a learning path, and build a lesson plan. That file references two supporting directories — read them from this same repo:

- `skills/eng-teaching/references/` — CEFR calibration, lesson frameworks, the proven activity bank, and interview rationale
- `~/eng-teaching-records/` — where per-student profiles, learning paths, and vocab logs are read from and written to (create it if it doesn't exist). This lives outside the repo/skill directory on purpose, so it survives updates to this repo — see Step 0 in `SKILL.md`.

One section, **Step 6: Export as a .docx**, names a specific plugin available on Claude Code. On any other harness: skip that step, or substitute whatever document-generation capability is actually available in that environment (a local `docx`/`python-docx` library, a built-in export tool) — the rest of the workflow doesn't depend on it.

**Platform notes:**
- **Codex, Aider, Cursor, Windsurf, RooCode/Cline, Gemini CLI, and other AGENTS.md-reading tools**: no setup needed — just have this repo checked out or open in the project, and follow `skills/eng-teaching/SKILL.md` as instructed above.
- **Claude Code**: install as a plugin instead (see `README.md`) — Claude Code plugins are namespaced and versioned, which this file's convention doesn't provide.
- **Anything else** (a custom system prompt, a custom-GPT instructions field, a bespoke agent framework): paste in the body of `skills/eng-teaching/SKILL.md` (everything below its YAML frontmatter) directly, along with the contents of `skills/eng-teaching/references/`.
