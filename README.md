# eng-teaching

A [Claude Code plugin](https://code.claude.com/docs/en/plugins) for one-on-one English (ESL/EFL) tutors. It interviews you about a student, designs a multi-lesson learning path (or a single lesson), and generates classroom-ready lesson plans — with personalized vocabulary, a proven activity bank, and per-student memory so it never re-teaches the same word twice.

## What it does

- **Interviews the tutor** about the student (name, pronouns, level, location, interests, goal, session length, activity preferences) — in a **detailed** mode (each question separate) or a **light** mode (a few bundled open-ended prompts)
- **Designs a learning path**: a lesson-by-lesson table (theme, grammar point, activities) that varies grammar and activities across the path and weights activities toward whatever skill the tutor wants to prioritize (e.g. speaking fluency)
- **Generates lesson plans** with a standard structure: objectives, vocab review (recycling older words), fresh vocabulary (pitched one level above the student, personalized to their interests), warm-up, presentation/practice, 2-3 production activities to choose from, wrap-up, and a home assignment (included by default, opt-out)
- **Remembers each student** in a plain-text record file — so asking for "the next lesson for Mar" in a brand-new session picks up exactly where you left off, without re-running the interview
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

## Optional: exporting to Word

Exporting a lesson plan to `.docx` uses the `docx` skill from Anthropic's `document-skills` plugin (in the [`anthropics/skills`](https://github.com/anthropics/skills) repo) — it isn't bundled in this repo. Without it, the skill still works and just gives you the plan as markdown. To enable export (tested against this exact command sequence):

```
claude plugin marketplace add anthropics/skills
claude plugin install document-skills@anthropic-agent-skills
```

Then ask for a Word/`.docx` version after any lesson plan is generated.

## Installation

This repo is a Claude Code **plugin** (not a raw skill folder) — install it directly from GitHub, no manual cloning into `~/.claude/skills/` required. This exact two-command sequence was tested against this repo's `.claude-plugin/marketplace.json`:

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

or just ask Claude to plan an English lesson (e.g. "plan a B1 speaking lesson for my student Mar") — the skill's description is written to trigger automatically on relevant requests too.

## Where student data lives

Student profiles, learning paths, and vocabulary logs are stored **outside** this plugin's install directory, at `~/eng-teaching-records/<student-name>.md` — one plain-markdown file per student. This is deliberate: it keeps personal student data out of the plugin's files, so updating, reinstalling, or upgrading this plugin never touches or risks a tutor's actual records.

**Do not commit `~/eng-teaching-records/` to this repository or any other.** It's tutor-specific personal data and doesn't belong in the skill's source.

## Structure

```
eng-teaching/
├── .claude-plugin/
│   └── plugin.json                   # plugin manifest
└── skills/
    └── eng-teaching/
        ├── SKILL.md                  # the skill's instructions
        └── references/
            ├── student-interview.md          # interview question rationale + L1-interference cheat sheet
            ├── cefr-levels.md                # CEFR level calibration (A1-C2)
            ├── lesson-frameworks.md          # PPP / TBL / ESA / receptive-skills frameworks
            ├── activity-bank.md              # generic fallback activities by skill type
            └── teacher-activities.md         # the tutor's own proven activities (customize this)
```

## License

MIT — see [LICENSE](LICENSE).
