# eng-teaching

An AI agent skill for one-on-one English (ESL/EFL) tutors, usable with Claude Code, OpenAI Codex, Cursor, Aider, and other AI coding-agent harnesses. It interviews you about a student, designs a multi-lesson learning path (or a single lesson), and generates classroom-ready lesson plans — with personalized vocabulary, a proven activity bank, and per-student memory so it never re-teaches the same word twice.

## What it does

- **Interviews the tutor** about the student (name, pronouns, level, location, interests, goal, session length, activity preferences) — in a **detailed** mode (each question separate) or a **light** mode (a few bundled open-ended prompts)
- **Designs a learning path**: a lesson-by-lesson table (theme, grammar point, activities) that varies grammar and activities across the path and weights activities toward whatever skill the tutor wants to prioritize (e.g. speaking fluency)
- **Generates lesson plans** with a standard structure: objectives, vocab review (recycling older words), fresh vocabulary (pitched one level above the student, personalized to their interests), warm-up, presentation/practice, 2-3 production activities to choose from, wrap-up, a home assignment (included by default, opt-out), and teacher's notes (integrated by default — pick end-only or off instead)
- **Student handout**: whenever teacher's notes are on, a second, notes-free copy of the plan for the student is included by default (opt out if you don't want it)
- **Remembers each student** in a plain-text record file — so asking for "the next lesson for [student name]" in a brand-new session picks up exactly where you left off, without re-running the interview
- **Never repeats vocabulary** already taught to a student, and recycles older vocab into new lessons for retention
- **Draws on a bank of proven activities** (`skills/eng-teaching/references/teacher-activities.md`) — role plays, debates, gallery tours, press conferences, and more — plus a generic fallback bank for anything that doesn't fit
- **Exports a finished lesson plan as a `.docx`** on request — a lesson plan is a document you print, edit, or hand to a co-teacher, not just chat text

Currently scoped to **one-on-one tutoring only**; group classes are intentionally out of scope (see `SKILL.md`).

## The interview

Before planning anything, the skill interviews you about the student — either **detailed** (each question asked separately) or **light** (a few bundled open-ended prompts), your choice. Either way it gathers:

- Student name (free text, never a suggested/example name) and pronouns (he/him, she/her, they/them)
- Current level (a CEFR estimate, or a description of what they can/can't do if you're not sure)
- Location — the actual city/country (e.g. "Barcelona," "Naples"), used both to personalize content and to silently anticipate likely native-language interference errors
- Interests, profession, hobbies
- Goal for learning English (exam, work, travel, immigration, general fluency)
- Session length (30 min / 60 min / other), asked separately from activity preferences
- Activity choice — random from the proven activities list below, specific activities you already have in mind, or "show me the list first"
- Single lesson or a multi-lesson learning path (if a path, how many lessons — you decide, the skill never suggests a number), key skills to weight, and anything to include or exclude

Vocab style/count and whether to include a home assignment are asked later, only once an actual lesson is being drafted — not during this initial interview. For a returning student with an existing record, the skill skips straight to a brief check-in instead of re-running any of this.

## Activity bank

The skill draws primarily on this proven set of activities (`skills/eng-teaching/references/teacher-activities.md`) when building the Production stage of a lesson, spacing repeats out across a learning path:

**Vocabulary & input** — Fresh vocabulary tied to interests, read a generated dialogue using new vocab, read a found article, describe a picture, use a short media clip, use data/charts to discuss trends

**Speaking & discussion** — Speaking practice discussion questions, sentence upgrades, role play, ranking scenario (4-5 items), impossible choice scenario, mini debates, devil's advocate, inbox activity (respond to messages), press conference

**Roleplay/scenario formats** — Gallery tour (describe artworks/visuals), respond to "Subway takes" in a randomly assigned tone, Hometown Hero newspaper interview

If nothing on this list fits, the skill falls back to a generic activity bank (`skills/eng-teaching/references/activity-bank.md`) organized by skill type (grammar/vocab/speaking/listening/reading/writing).

Almost everything here is **generated** (dialogues, stories, scenarios, discussion questions) rather than requiring you to go find real material — "Describe a picture," "Gallery tour," and the data/charts activity default to a vivid written description or a generated data table instead of an actual photo, artwork, or chart. Only two activities (a real article, a real media clip) inherently need you to source something yourself, and the skill caps those at one per lesson at most.

## Optional: exporting to Word

Works the same way on **any harness with shell access** (Codex, Cursor, Aider, Claude Code, etc.) — no plugin install needed. The skill writes the finished plan to markdown, then runs the bundled converter:

```bash
pip install python-docx   # one-time, if not already installed
python3 skills/eng-teaching/scripts/md_to_docx.py <plan>.md <output>.docx
```

`md_to_docx.py` (tested end-to-end) converts headings, `**bold**`/`*italic*` text, bullet/numbered lists, and `> ` blockquote lines — used for Teacher's Notes, which render indented, gray, and italic so they read as clearly separate from student-facing content. Just ask for a Word/`.docx` version after any lesson plan is generated.

On Claude Code specifically, the `docx` skill from Anthropic's `document-skills` plugin (in [`anthropics/skills`](https://github.com/anthropics/skills)) is an alternative if you already have it installed, with more elaborate formatting options — but it's not required.

## Using this with other LLM harnesses (Codex, Cursor, Aider, etc.)

The actual instructions in `skills/eng-teaching/SKILL.md` are plain markdown with no Claude-specific syntax, so this works with any AI coding agent, not just Claude Code — including the `.docx` export above. This repo includes an [`AGENTS.md`](AGENTS.md) — a convention read automatically by OpenAI Codex, Cursor, Aider, Windsurf, Gemini CLI, RooCode/Cline, and several other harnesses. With this repo checked out (or its contents copied into your project), those tools pick it up with no extra setup.

For anything else — a custom system prompt, a custom-GPT instructions field, a bespoke agent — paste in `skills/eng-teaching/SKILL.md`'s body (below its YAML frontmatter) along with `skills/eng-teaching/references/` and `skills/eng-teaching/scripts/`. See `AGENTS.md` for the per-platform notes.

## Installing on Claude Code

This repo is also a Claude Code **plugin** (not a raw skill folder) — install it directly from GitHub, no manual cloning into `~/.claude/skills/` required. This exact two-command sequence was tested against this repo's `.claude-plugin/marketplace.json`:

```
claude plugin marketplace add ratherlegit/eng-teaching
claude plugin install eng-teaching@eng-teaching
```

**For local development/testing** (working on a clone of this repo):
```bash
claude plugin marketplace add /path/to/local/eng-teaching
claude plugin install eng-teaching@eng-teaching
```

Once installed, plugin skills are namespaced as `/plugin-name:skill-name` (confirmed by running it locally):

```
/eng-teaching:eng-teaching
```

or just ask Claude to plan an English lesson (e.g. "plan a B1 speaking lesson for my student [student name]") — the skill's description is written to trigger automatically on relevant requests too.

## Where student data lives

Student profiles, learning paths, and vocabulary logs are stored **outside** this plugin's install directory, at `~/eng-teaching-records/<student-name>.md` — one plain-markdown file per student. This is deliberate: it keeps personal student data out of the plugin's files, so updating, reinstalling, or upgrading this plugin never touches or risks a tutor's actual records.

**Do not commit `~/eng-teaching-records/` to this repository or any other.** It's tutor-specific personal data and doesn't belong in the skill's source.

## Structure

```
eng-teaching/
├── AGENTS.md                         # entry point for Codex/Cursor/Aider/etc.
├── .claude-plugin/
│   ├── plugin.json                   # plugin manifest (Claude Code)
│   └── marketplace.json              # marketplace manifest (Claude Code)
└── skills/
    └── eng-teaching/
        ├── SKILL.md                  # the skill's instructions
        ├── scripts/
        │   └── md_to_docx.py          # harness-agnostic markdown -> .docx converter
        └── references/
            ├── student-interview.md          # interview question rationale + L1-interference cheat sheet
            ├── cefr-levels.md                # CEFR level calibration (A1-C2)
            ├── lesson-frameworks.md          # PPP / TBL / ESA / receptive-skills frameworks
            ├── activity-bank.md              # generic fallback activities by skill type
            └── teacher-activities.md         # the tutor's own proven activities (customize this)
```

## License

MIT — see [LICENSE](LICENSE).
