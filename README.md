# eng-teaching

**A planning assistant for one-on-one English tutors.** Tell it about your student once, and it interviews you, drafts a personalized lesson (or a whole multi-lesson course), and remembers everything for next time, so you spend your prep time refining a strong starting point instead of building one from a blank page.

Think of it as a fast, well-organized first draft, not a replacement for your judgment. Every activity, question, and note is meant to be reviewed, edited, or swapped. You're always the one deciding what actually happens in the room.

It works inside Claude (Claude.ai, the Claude apps, or Claude Code), ChatGPT, and a few other AI assistants. See [Setting it up](#setting-it-up) below.

## Why tutors use this

- **It saves you the hours of prep, not the decisions.** It asks about your student once (their level, interests, goals, how long your sessions are) and hands you a complete, well-structured lesson built around them, ready for you to adjust, cut, or build on however you see fit.
- **It never forgets what you've already taught.** Every lesson checks what vocabulary this student already knows, so you're never accidentally repeating a word they learned three lessons ago, and you can spend your review time on what actually needs reinforcing.
- **It drafts the content, so you're rarely starting from a blank page.** Dialogues, discussion questions, role-play scenarios, reading passages: all drafted fresh and tied to your student's actual interests, ready for you to use as-is or make your own.
- **It plans ahead, not just one lesson at a time.** For example, ask for a 10-lesson course and it lays out the whole path (themes, grammar points, activities) before writing a single lesson, so you can see where things are headed and adjust the plan before it builds anything.
- **You get a real, editable document.** Every lesson plan can be exported as a `.docx` file you can open in Word or Google Docs to make it truly your own: print it, mark it up, or hand it to a co-teacher.
- **It knows the difference between what you need and what your student needs.** It can write pedagogical notes just for you (why an activity was chosen, what to watch out for) and, separately, a clean copy for the student with none of that on it.
- **It picks up right where you left off.** Come back next week and say "next lesson for [student]," and it already knows their level, their path, and everything they've learned so far. No re-explaining, and it'll check in with you before assuming the plan hasn't changed.

## How a session works

1. **You tell it about your student.** Name, level, interests, goal, how long your sessions run. You can answer a handful of quick questions or a few short open-ended prompts, your choice.
2. **If you want more than one lesson, it plans the whole path first.** You'll see a table of every lesson's theme and focus before anything gets written in full, so you can ask for changes.
3. **It drafts the lesson.** An objective, a warm-up conversation, a vocabulary section pitched to stretch your student a little, a grammar or skill focus, a couple of activity options for you to choose between live, a wrap-up, and (if you want one) a take-home assignment. Treat it as your starting point: swap an activity, add your own, cut what doesn't fit your student.
4. **You can ask for a clean student copy.** A second version of the same lesson, with your private notes removed, ready to hand or send to the student.
5. **You can export it as a Word document.** Ask for a `.docx` version any time and it'll build one you can save, mark up, and reshape into exactly what you want to teach.
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

**Almost everything is drafted for you, so you're rarely stuck hunting for materials.** Dialogues, stories, discussion prompts, even described images or data charts are all written fresh rather than something you have to go track down, and every one of them is easy to edit or replace if you'd rather use your own. The only exceptions are a real article or a real photo, video, or audio clip, and even those are capped at one per lesson, only used when nothing drafted would do the job as well (like exam prep that needs authentic text).

## Setting it up

Most tutors will use one of the first two options below. Pick whichever assistant you already have.

### Claude.ai, the Claude Desktop app, or the Claude mobile app (most tutors)

No installation. This uses Claude's built-in **Projects** feature, which is available on every Claude plan, including free:

1. On [claude.ai](https://claude.ai) (or in the Claude Desktop/mobile app, which use the same account), create a new **Project**, something like "English Tutoring."
2. Open the project's settings and paste the contents of [`skills/eng-teaching/SKILL.md`](skills/eng-teaching/SKILL.md) into the **custom instructions** field.
3. Upload the files inside [`skills/eng-teaching/references/`](skills/eng-teaching/references/) to the project's **knowledge** files.
4. Start any chat inside that project and ask for a lesson plan, e.g. *"Plan a B1 speaking lesson for my student Maria."* Every chat in the project will follow the skill automatically.

*If you'd rather not set up a project, the simplest path works everywhere: open a new chat, paste the contents of `SKILL.md` at the very top of your first message, and ask your question underneath it. It's a bit more typing each time, but there's nothing to configure.*

### ChatGPT

Also uses no installation, via ChatGPT's **Projects** feature (available on every ChatGPT plan, including free):

1. In ChatGPT, create a new **Project**.
2. Open its settings and paste the contents of `skills/eng-teaching/SKILL.md` into the project's **instructions**.
3. Upload the files inside `skills/eng-teaching/references/` as the project's files.
4. Chat inside that project as normal, asking for lesson plans the same way as above.

The same simplest-path fallback applies here too: paste `SKILL.md`'s contents at the top of a new chat if you'd rather skip project setup.

### Claude Code (technical users)

If you use Claude Code, the AI assistant that runs in a terminal/command line, this is a plugin you can install directly:

```
claude plugin marketplace add ratherlegit/eng-teaching
claude plugin install eng-teaching@eng-teaching
```

Then just ask, in plain English: *"Plan a B1 speaking lesson for my student Maria."* You don't need to remember any special commands. Claude will recognize what you're asking for. (If you want to invoke it explicitly, the command is `/eng-teaching:eng-teaching`.)

*If those two lines above don't mean anything to you, that's completely fine. This step just needs to be done once, and a technical friend or colleague can run those two commands for you in a couple of minutes. After that, using the skill is just a normal conversation.*

### Other AI coding assistants

If you're using OpenAI Codex, Cursor, Aider, Windsurf, Gemini CLI, or several others, this repo includes a file called `AGENTS.md` that those tools read automatically. Just have this repo open in your project and ask for a lesson plan as normal. No installation step needed.

## Getting your lesson plan as a Word document

Just ask. After any lesson plan is generated, say something like *"can I get that as a Word doc?"*

**On Claude Code or another coding assistant**, it'll ask where you'd like the file saved (if it doesn't already know), then hand you a real `.docx` file you can open, mark up, and reshape however you'd like. If a student handout was made too, that comes as its own separate file. It'll always tell you exactly where the file ended up, or share it directly in the chat if there's nowhere else to put it.

**On Claude.ai, the Claude apps, or ChatGPT without code execution enabled**, there's no way for the assistant to run our export script directly, so ask it to format the plan so it pastes cleanly, then copy it straight into Word or Google Docs; the headings, bold text, and lists carry over well. If your ChatGPT plan has Python/code execution available, you can instead ask it to run the same conversion approach we use elsewhere in this repo.

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
