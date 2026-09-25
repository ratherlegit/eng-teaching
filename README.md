# eng-teaching

**A planning assistant for one-on-one English tutors.** Tell it about your student once, and it interviews you, builds a personalized lesson (or a whole multi-lesson course), and remembers everything for next time, so you're not starting from a blank page every session.

It works inside Claude (Claude.ai, the Claude apps, or Claude Code) and a few other AI assistants. See [Setting it up](#setting-it-up) below.

## Why tutors use this

- **No more starting from scratch.** It asks about your student once (their level, interests, goals, how long your sessions are) and remembers it for every future lesson.
- **It never forgets what you've already taught.** Every lesson checks what vocabulary this student already knows, so you're never accidentally repeating a word they learned three lessons ago.
- **It writes almost everything for you.** Dialogues, discussion questions, role-play scenarios, reading passages: all generated fresh and tied to your student's actual interests. You almost never have to go find your own materials.
- **It plans ahead, not just one lesson at a time.** For example, ask for a 10-lesson course and it lays out the whole path (themes, grammar points, activities) before writing a single lesson, so you can see where things are headed and adjust before it builds anything.
- **You get a real, editable document.** Every lesson plan can be exported as a `.docx` file you can open in Word or Google Docs, print, tweak, or hand to a co-teacher.
- **It knows the difference between what you need and what your student needs.** It can write pedagogical notes just for you (why an activity was chosen, what to watch out for) and, separately, a clean copy for the student with none of that on it.
- **It picks up right where you left off.** Come back next week and say "next lesson for [student]," and it already knows their level, their path, and everything they've learned so far. No re-explaining.

## How a session works

1. **You tell it about your student.** Name, level, interests, goal, how long your sessions run. You can answer a handful of quick questions or a few short open-ended prompts, your choice.
2. **If you want more than one lesson, it plans the whole path first.** You'll see a table of every lesson's theme and focus before anything gets written in full, so you can ask for changes.
3. **It builds the lesson.** An objective, a warm-up conversation, a vocabulary section pitched to stretch your student a little, a grammar or skill focus, a couple of activity options for you to choose between live, a wrap-up, and (if you want one) a take-home assignment.
4. **You can ask for a clean student copy.** A second version of the same lesson, with your private notes removed, ready to hand or send to the student.
5. **You can export it as a Word document.** Ask for a `.docx` version any time and it'll build one you can save, print, or edit.
6. **It remembers the student for next time.** Their profile, their course plan, and every word they've learned so far are saved automatically, so future sessions never start from zero.

## What's inside a lesson plan

- **Objective**, followed right away by a **Warm Up Conversation**: a few genuine, friendly check-in questions to open the lesson (fresh every time, never the same script twice)
- **Vocab review**: a quick refresher on older words, so nothing taught in past lessons gets forgotten
- **Fresh vocabulary**: new words or phrases tied to the topic, pitched to gently stretch the student above their current level
- **Grammar Focus**: the lesson's main teaching point, with practice
- **Production activities**: 2 to 3 options (role play, debate, discussion, and more) so you can pick or swap live depending on how the lesson is going
- **Wrap-up**, and an optional **home assignment**
- **A Time Summary** with suggested timings, a guide rather than a strict schedule
- **Teacher's notes** (optional): private pointers just for you, like why an activity was picked, what might trip this student up, or timing tips. Only shown where there's genuinely something worth flagging, never padded in for its own sake.

## The activities it draws from

Most activities come from a bank of classroom-tested formats: role plays, debates, ranking exercises, mock press conferences, "gallery tours" of described artwork, newspaper-style interviews, and more. If nothing on that list fits the lesson, it falls back to a broader general-purpose activity bank organized by skill (grammar, speaking, listening, reading, writing).

**Almost everything is generated for you.** Dialogues, stories, discussion prompts, even described images or data charts are all written fresh rather than something you have to go track down. The only exceptions are a real article or a real photo, video, or audio clip, and even those are capped at one per lesson, only used when nothing generated would do the job as well (like exam prep that needs authentic text).

## Setting it up

**If you're using Claude Code, the AI assistant that runs in a terminal/command line**, this is a plugin you can install directly:

```
claude plugin marketplace add ratherlegit/eng-teaching
claude plugin install eng-teaching@eng-teaching
```

Then just ask, in plain English: *"Plan a B1 speaking lesson for my student Maria."* You don't need to remember any special commands. Claude will recognize what you're asking for. (If you want to invoke it explicitly, the command is `/eng-teaching:eng-teaching`.)

*If those two lines above don't mean anything to you, that's completely fine. This step just needs to be done once, and a technical friend or colleague can run those two commands for you in a couple of minutes. After that, using the skill is just a normal conversation.*

**If you're using a different AI coding assistant** (OpenAI Codex, Cursor, Aider, Windsurf, Gemini CLI, and several others), this repo includes a file called `AGENTS.md` that those tools read automatically. Just have this repo open in your project and ask for a lesson plan as normal. No installation step needed.

**If you're pasting this into something else entirely** (a custom chatbot, a GPT, anywhere else), open `skills/eng-teaching/SKILL.md` and paste its contents (everything below the top few lines) into that tool's instructions, along with the files in `skills/eng-teaching/references/`.

## Getting your lesson plan as a Word document

Just ask. After any lesson plan is generated, say something like *"can I get that as a Word doc?"* The skill will ask where you'd like it saved (if it doesn't already know), then hand you a real `.docx` file you can open, edit, and print. If a student handout was made too, that comes as its own separate file. It'll always tell you exactly where the file ended up, or share it directly in the chat if there's nowhere else to put it.

This works the same way across every supported AI assistant. No extra plugin required.

## Where your student's information is kept

Everything about your students (their profile, their course plan, and the words they've been taught) is saved in plain text files on your own computer, at `~/eng-teaching-records/`, completely separate from the skill itself. That means updating or reinstalling the skill never touches your students' records.

**If you're maintaining a copy of this repo yourself, don't upload that folder to GitHub or share it anywhere.** It's your students' personal information and shouldn't leave your computer.

## For developers

<details>
<summary>Repo structure, technical details, and contributing notes</summary>

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

**Local development/testing** (working on a clone of this repo):
```bash
claude plugin marketplace add /path/to/local/eng-teaching
claude plugin install eng-teaching@eng-teaching
```

**The `.docx` export** runs `python3 skills/eng-teaching/scripts/md_to_docx.py <plan>.md <output>.docx` (requires `pip install python-docx`). It converts markdown headings, bold/italic text, bullet/numbered lists, tables, and `> ` blockquote lines (used for Teacher's Notes, rendered indented/gray/italic) into a real `.docx`. Works identically on any harness with shell access (Codex, Cursor, Aider, Claude Code, etc.), no plugin install needed. On Claude Code specifically, Anthropic's `docx` skill (from the `document-skills` plugin in [`anthropics/skills`](https://github.com/anthropics/skills)) is an alternative if already installed, with more elaborate formatting via docx-js, but the bundled script is the default since it needs no extra install.

**Cross-harness support**: the actual instructions in `skills/eng-teaching/SKILL.md` are plain markdown with no Claude-specific syntax. `AGENTS.md` is a real, widely-adopted convention (see [agents.md](https://agents.md)) read automatically by OpenAI Codex, Cursor, Aider, Windsurf, Gemini CLI, RooCode/Cline, and others.

Currently scoped to **one-on-one tutoring only**; group classes are intentionally out of scope (see `SKILL.md`).

</details>

## License

MIT. See [LICENSE](LICENSE).
