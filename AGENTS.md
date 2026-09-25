# eng-teaching

An ESL/EFL lesson-planning agent: interviews a tutor about a student, designs a multi-lesson learning path, and generates classroom-ready lesson plans with personalized vocabulary and per-student memory. This file follows the [AGENTS.md](https://agents.md) convention, read automatically by OpenAI Codex, Cursor, Aider, Windsurf, Gemini CLI, RooCode/Cline, and several other coding-agent harnesses — no platform-specific setup needed beyond having this repo (or its contents) available to read.

**Follow the instructions in `skills/eng-teaching/SKILL.md`** for the full workflow: how to interview a tutor, design a learning path, and build a lesson plan. That file references two supporting directories — read them from this same repo:

- `skills/eng-teaching/references/` — CEFR calibration, lesson frameworks, the proven activity bank, and interview rationale
- `~/eng-teaching-records/` — where per-student profiles, learning paths, and vocab logs are read from and written to (create it if it doesn't exist). This lives outside the repo/skill directory on purpose, so it survives updates to this repo — see Step 0 in `SKILL.md`.

**Step 6: Export as a .docx** works on any harness with shell access: it runs the bundled `skills/eng-teaching/scripts/md_to_docx.py` script (requires `pip install python-docx`, tested end-to-end) to convert the finished plan into a real `.docx` file — no platform-specific plugin required. In a chat-based harness where filesystem access depends on a connected folder (rather than always-on shell access), don't assume last session's connection carried over or that a script-written file will reach the tutor automatically — verify a working delivery path first (see SKILL.md's Step 6 for the full fallback flow, including delivering the file directly in the chat if no folder is connected).

**Platform notes:**
- **Codex, Aider, Cursor, Windsurf, RooCode/Cline, Gemini CLI, and other AGENTS.md-reading tools**: no setup needed — just have this repo checked out or open in the project, and follow `skills/eng-teaching/SKILL.md` as instructed above.
- **Claude Code**: install as a plugin instead (see `README.md`) — Claude Code plugins are namespaced and versioned, which this file's convention doesn't provide.
- **Anything else** (a custom system prompt, a custom-GPT instructions field, a bespoke agent framework): paste in the body of `skills/eng-teaching/SKILL.md` (everything below its YAML frontmatter) directly, along with the contents of `skills/eng-teaching/references/`. The `.docx` export step needs shell/code-execution access to run the script — without it, the plan is still fully usable as markdown.
