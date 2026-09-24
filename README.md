# eng-teaching

A [Claude Code skill](https://docs.claude.com/en/docs/claude-code/skills) for one-on-one English (ESL/EFL) tutors. It interviews you about a student, designs a multi-lesson learning path (or a single lesson), and generates classroom-ready lesson plans — with personalized vocabulary, a proven activity bank, and per-student memory so it never re-teaches the same word twice.

## What it does

- **Interviews the tutor** about the student (name, pronouns, level, location, interests, goal, session length, activity preferences) — in a **detailed** mode (each question separate) or a **light** mode (a few bundled open-ended prompts)
- **Designs a learning path**: a lesson-by-lesson table (theme, grammar point, activities) that varies grammar and activities across the path and weights activities toward whatever skill the tutor wants to prioritize (e.g. speaking fluency)
- **Generates lesson plans** with a standard structure: objectives, vocab review (recycling older words), fresh vocabulary (pitched one level above the student, personalized to their interests), warm-up, presentation/practice, 2-3 production activities to choose from, wrap-up, and an optional home assignment
- **Remembers each student** in a plain-text record file — so asking for "the next lesson for Mar" in a brand-new session picks up exactly where you left off, without re-running the interview
- **Never repeats vocabulary** already taught to a student, and recycles older vocab into new lessons for retention
- **Draws on a bank of proven activities** (`references/teacher-activities.md`) — role plays, debates, gallery tours, press conferences, and more — plus a generic fallback bank for anything that doesn't fit

Currently scoped to **one-on-one tutoring only**; group classes are intentionally out of scope (see `SKILL.md`).

## Installation

Copy this folder into your Claude Code skills directory:

```bash
git clone https://github.com/<your-username>/eng-teaching.git ~/.claude/skills/eng-teaching
```

Or, for a single project instead of globally, clone it into that project's `.claude/skills/` directory.

Once installed, invoke it from Claude Code with:

```
/eng-teaching
```

or just ask Claude to plan an English lesson — the skill's description is written to trigger automatically on relevant requests.

## Where student data lives

Student profiles, learning paths, and vocabulary logs are stored **outside** this skill's folder, at `~/eng-teaching-records/<student-name>.md` — one plain-markdown file per student. This is deliberate: it keeps personal student data out of the skill's installation directory, so updating, reinstalling, or re-cloning this skill never touches or risks a tutor's actual records.

**Do not commit `~/eng-teaching-records/` to this repository or any other.** It's tutor-specific personal data and doesn't belong in the skill's source.

## Using this in a shared/team workspace ("cowork")

If multiple tutors share a Claude Code environment (a shared machine, a shared project, or a team account) rather than each running their own local install:

- **Records are per-machine, under each person's home directory** (`~/eng-teaching-records/`). Two tutors on the same shared machine, each with their own user account, naturally keep separate student records — nothing to configure.
- **If a team genuinely wants to share student records** (e.g. two tutors co-teaching the same student, or handing off a student between tutors), sync or share the `~/eng-teaching-records/<student>.md` file directly — it's a single plain-text file, so it works fine in a shared drive, a private git repo, or a team wiki page. Don't put it in this skill's own repo.
- **The skill itself (`SKILL.md` and `references/`) is safe to share/install identically across a whole team** — it contains no student data, only the teaching logic and the tutor's own proven-activities list. If your team's proven activities differ from the ones checked in here, fork this repo and edit `references/teacher-activities.md` to match your own list.
- Treat any student data (names, interests, locations, vocab logs) as sensitive — avoid pasting full record files into shared channels or public issues when asking for support.

## Structure

```
eng-teaching/
├── SKILL.md                          # the skill's instructions
└── references/
    ├── student-interview.md          # interview question rationale + L1-interference cheat sheet
    ├── cefr-levels.md                # CEFR level calibration (A1-C2)
    ├── lesson-frameworks.md          # PPP / TBL / ESA / receptive-skills frameworks
    ├── activity-bank.md              # generic fallback activities by skill type
    └── teacher-activities.md         # the tutor's own proven activities (customize this)
```

## License

MIT — see [LICENSE](LICENSE).
