# eng-teaching

A [Claude Code plugin](https://code.claude.com/docs/en/plugins) for one-on-one English (ESL/EFL) tutors. It interviews you about a student, designs a multi-lesson learning path (or a single lesson), and generates classroom-ready lesson plans — with personalized vocabulary, a proven activity bank, and per-student memory so it never re-teaches the same word twice.

## What it does

- **Interviews the tutor** about the student (name, pronouns, level, location, interests, goal, session length, activity preferences) — in a **detailed** mode (each question separate) or a **light** mode (a few bundled open-ended prompts)
- **Designs a learning path**: a lesson-by-lesson table (theme, grammar point, activities) that varies grammar and activities across the path and weights activities toward whatever skill the tutor wants to prioritize (e.g. speaking fluency)
- **Generates lesson plans** with a standard structure: objectives, vocab review (recycling older words), fresh vocabulary (pitched one level above the student, personalized to their interests), warm-up, presentation/practice, 2-3 production activities to choose from, wrap-up, and an optional home assignment
- **Remembers each student** in a plain-text record file — so asking for "the next lesson for Mar" in a brand-new session picks up exactly where you left off, without re-running the interview
- **Never repeats vocabulary** already taught to a student, and recycles older vocab into new lessons for retention
- **Draws on a bank of proven activities** (`references/teacher-activities.md`) — role plays, debates, gallery tours, press conferences, and more — plus a generic fallback bank for anything that doesn't fit

Currently scoped to **one-on-one tutoring only**; group classes are intentionally out of scope (see `SKILL.md`).

## Installation

This repo is a Claude Code **plugin** (not a raw skill folder) — install it directly from GitHub, no manual cloning into `~/.claude/skills/` required.

**Claude Code v2.1.275+:**
```
/plugin install eng-teaching --marketplace ratherlegit/eng-teaching
```

**Older Claude Code versions:**
```
/plugin marketplace add ratherlegit/eng-teaching
/plugin install eng-teaching@ratherlegit-eng-teaching
```

**For local development/testing** (working on this repo itself):
```bash
claude --plugin-dir ./eng-teaching
```

Once installed, invoke it from Claude Code with:

```
/eng-teaching
```

or just ask Claude to plan an English lesson — the skill's description is written to trigger automatically on relevant requests.

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
