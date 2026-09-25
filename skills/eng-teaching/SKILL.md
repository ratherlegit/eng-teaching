---
name: eng-teaching
description: Develops lesson plans for teaching English to ESL/EFL learners (non-native speakers, general English or exam prep). Use when the user asks to create, draft, or plan an English lesson, class, or unit — e.g. "write a lesson plan for present perfect", "plan a B1 speaking class", "I need a 45-minute ESL lesson on phrasal verbs", or "help me teach conditionals to intermediate students". Covers grammar, vocabulary, speaking, listening, reading, and writing lessons across CEFR levels A1-C2, and can build single lessons or multi-lesson units.
---

# ESL Lesson Plans

Build classroom-ready English lesson plans for ESL/EFL learners. Adapt structure, depth, and format to what the user asks for — don't force a template they didn't request.

**This skill covers one-on-one tutoring only.** Group classes need different personalization logic (shared themes instead of one student's interests, per-student differentiation, etc.) and are intentionally out of scope for now — treat any group-class request as a case for a separate skill rather than stretching this one to fit.

## Step 0: Check for a returning student

Before interviewing anyone, check whether the tutor is naming a student you already have a record for — e.g. "give me the next lesson for [student name]," or any request naming a student by name. Look for `~/eng-teaching-records/<name-slug>.md` (lowercase, hyphens for spaces).

**Records live outside the skill's own folder, at `~/eng-teaching-records/`, not inside `~/.claude/skills/eng-teaching/`.** This is deliberate: a student's profile, path, and vocab history are personal data that must survive skill updates, reinstalls, or repackaging — they must never be at risk of being wiped out along with the skill's own files. Create `~/eng-teaching-records/` if it doesn't exist yet.

- **If a record exists**: skip Step 1 entirely — do not re-run the interview. Load the student's profile, learning path (if any), and vocab log straight from that file. Ask only a brief check-in: whether anything's changed (interests, exclusions, session length, etc.) and, if there's an active path, confirm you're generating the next undelivered lesson (the record tracks this — don't ask the tutor to specify a lesson number). Then go straight to Step 3/4 to build that lesson.
- **If no record exists** for that name, or this is the first time this student is mentioned: run the full Step 1 interview as normal, then create the record at the end of Step 1 (and update it after Step 2/4, per the Student Record File section below).

This is what makes "next lesson for [student name]" work in a brand-new session without re-asking their level, interests, goal, or path from scratch.

## Step 1: Interview the tutor

Before drafting, ask the tutor about the student — this is what turns a generic plan into one that actually fits the learner. Don't skip this for a one-off "just give me a lesson on X" request; ask briefly first, unless Step 0 found an existing record for this student.

**First, ask whether they want a detailed or light interview, with detailed as the default.** Detailed asks each item as its own question; light bundles the same information into a handful of open-ended prompts. Both gather the same information — light is just faster to answer, at the cost of the tutor doing more free-text writing per prompt instead of picking from options. If the tutor doesn't express a preference, proceed with detailed.

### Detailed mode
Ask for the following as **separate questions**, in this order — don't merge distinct pieces of information into one combined question, even when batching several questions into one round:
- **Student name**, asked first. This is always **free text entered by the tutor** — never offer suggested/example names as options; a name isn't a preference to pick from a list
- **Pronouns**, asked second — he/him, she/her, or they/them. Use this consistently in the plan rather than defaulting to "they" or guessing from the name
- **Current level** — CEFR estimate if known, or "not sure" (offer to help gauge it from a description of what they can/can't do)
- **Location/hometown or country** — ask for the specific place the student is from (city and/or country, e.g. "Barcelona" or "Naples"), not a language-family multiple choice. This is a free-text question like the student's name. Use the specific place as real personalization material (weave in local references, regional context, culturally relevant topics — a Barcelona student and a Naples student shouldn't get interchangeable "European" content). Silently infer the native language and likely L1-interference patterns from the location using `references/student-interview.md`'s cheat sheet — this informs your error-anticipation without needing to surface CEFR/linguistics jargon back to the tutor
- **Interests / profession / hobbies** — used to personalize example sentences and topics, which drives engagement far more than generic content
- **Goal for learning English** — exam, travel, work, immigration, general fluency — this should steer topic and skill choice as much as the level does
- **Session length** — its own question, distinct from activity preferences. Offer concrete options such as 30 minutes, 60 minutes, or "something else" (let the tutor specify), and ask separately about frequency if relevant
- **Activity choice** — its own question. The tutor likely doesn't know what's in `references/teacher-activities.md`, so don't just ask "random or specific" cold. Offer three options: (a) **random** — you pick from the proven activities list each time, (b) the tutor **already has specific activities in mind** (their own, not necessarily from the list) and will name them, or (c) **show me the list first** — in which case display the activity names from `references/teacher-activities.md` (names alone are enough, not the full descriptions), then re-ask whether they want random selection or want to name specific ones now that they've seen it
- **Single lesson or learning path** — ask only whether it's one lesson or a multi-lesson path. **If a path is chosen, immediately ask how many lessons as the very next question**, before moving on to anything else — don't leave it implicit or bundle it with later questions, and never suggest or default to a specific number
- **Key skills to focus on** — if a path, which skills should get the most weight (e.g. speaking fluency, exam writing, listening comprehension) — this biases activity/theme selection across the whole path, not just one lesson
- **Anything else to incorporate or exclude** — e.g. topics to avoid (sensitive for this student), a coursebook/curriculum to align with, grammar points already mastered (skip re-teaching) or specifically requested

### Light mode
Same information, bundled into four open-ended prompts instead of ~11 separate questions. Ask them one at a time, in order — each answer can be a short free-text paragraph covering multiple things at once:
1. "What's the student's name, pronouns, and current level (or describe what they can/can't do if you're not sure)?"
2. "Where are they from, and what are their interests, profession, or hobbies?"
3. "What's their goal for learning English, and how long are your sessions (30 min, 60 min, other)?"
4. "Is this a single lesson or a learning path (if a path, how many lessons)? And any preferences for activities or things to include/exclude — or just say 'you choose' for defaults."

Parse each free-text answer into the same underlying fields as detailed mode (name, pronouns, level, location, interests, goal, session length, single/path + count, activity choice, key skills, exclusions) before moving to Step 2 — light mode changes how the questions are asked, not what ends up stored in the record. If an answer leaves something out or is ambiguous, ask a quick one-off follow-up for just that item rather than guessing.

Either mode: **do not ask about vocab preferences or home assignments in this initial round.** Those only matter once an actual lesson is being written (see Step 4) — a tutor who's just asking for a learning path shouldn't have to answer questions about a single lesson's vocab section yet.

See `references/student-interview.md` for the full question set, why each matters, and an L1-interference cheat sheet to use once you know the student's location (map it to a likely native language yourself).

For a returning student with an existing record (see Step 0), don't run this interview at all beyond the brief check-in described there.

## Step 2: Design the learning path

Skip this step only when the tutor asked for a single standalone lesson. Whenever more than one lesson is requested, design the full path **before** writing any individual lesson plan, and share it with the tutor for approval/adjustment before drafting lesson content.

Output the path as a table:

| Lesson # | Lesson Name | Theme | Grammar Point | Activity Type(s) |
|---|---|---|---|---|
| 1 | ... | ... | ... | ... |

The Activity Type(s) column can list 2-3 activities per lesson, since Step 4 builds out that many for the Production stage — the path table is just the preview of what each lesson will offer.

Rules for building the path:
- **Vary grammar points** across lessons, all appropriate to the student's level — don't drill one structure for the whole path unless the tutor specifically asked for that. Use `references/cefr-levels.md` to keep every grammar point within (or just at the edge of) the student's level, and sequence roughly from simpler to more complex where there's a natural order (e.g. present simple before present perfect before third conditional)
- **Vary activity types** across the path, pulling primarily from `references/teacher-activities.md`. Repetition of an activity is fine in longer paths, but space repeats out — don't repeat an activity within at least 3-4 lessons of its last use, and never back-to-back
- **Keep external-material activities rare across the path too** — default to the generated, self-contained activities (see "Choosing the activities" in Step 4) and don't schedule the found-article/media-clip activities back-to-back or in most lessons; one every few lessons at most, only where it clearly fits
- **Weight toward the tutor's stated key skills** — if speaking fluency was flagged as the focus, more lessons should land on speaking-heavy activities (Role Play, Mini Debates, Press conference, etc.) than on receptive-only ones, without abandoning variety entirely
- **Respect exclusions** — never schedule an excluded topic or a grammar point the tutor said is already mastered or off-limits
- Themes should track the student's interests and goal, varying enough that the path doesn't feel repetitive, while still building toward the stated overall goal

**Do not generate any lesson plans immediately after presenting the path.** After sharing the path table, ask the tutor whether they'd like any adjustments to it, or would like Lesson 1 generated. Only proceed to Step 3/4 for Lesson 1 once they say so — never generate the whole path's lessons in one go, and never generate even Lesson 1 before this check-in. Once Lesson 1 is delivered, ask before generating the next one, following Steps 3-4 for each using that lesson's row as the spec (theme, grammar point, activity type) rather than re-deciding those from scratch.

Once the tutor approves the path (even before any lesson is generated), save it into the student's record file (see Step 4's Student Record File section) with a Status column (all rows start "Not yet delivered"). This is what lets a future session pick up with "next lesson for [student name]" without the path having to be re-designed or re-approved.

Once the path is approved, also ask the tutor whether they'd like the path table exported as a `.docx` — it's a handy reference file to save outside the chat (e.g. to share with the student or a co-teacher, or to check off lessons as they're delivered). If yes, follow Step 6 to build it. This is separate from, and in addition to, the per-lesson export offer in Step 6.

## Step 3: Pick a framework

Default to **PPP (Presentation-Practice-Production)** for grammar/vocab lessons unless the user asks for something else or the goal is clearly task-based (e.g. exam prep, project work) or skills-focused (e.g. a reading/listening lesson, which fits better as pre-/during-/post-task).

See `references/lesson-frameworks.md` for framework options (PPP, TBL, ESA, receptive-skills pre/during/post) and when each fits.

## Step 4: Build the plan

If this lesson comes from an approved learning path, use that row's theme, grammar point, and activity type as given rather than re-choosing them — Step 4 is about fleshing them out into a full plan, not re-deciding the fundamentals.

### Lesson-specific questions (ask now, not during Step 1)
Before drafting, if not already answered for this student, ask:
- **Vocab constraint** — any preference for the fresh vocabulary section: phrasal verbs specifically, a certain part of speech (nouns, adjectives, etc.), or no constraint (default: mixed, chosen to fit the topic/interest)
- **Number of vocab words/phrases** — how many items to include (default: 6-10 if the tutor doesn't specify)
- **Home assignment** — whether to include a take-home assignment section (default: include, unless the tutor opts out)
- **Teacher's notes** — pedagogical rationale, timing/pacing tips, anticipated difficulties, and alternative approaches for the tutor's own reference (not shown to the student). Ask whether these should be **integrated** throughout the plan (a short note under each relevant stage), **end-only** (a single "Teacher's Notes" section after everything else), or **not included** at all. Default: **integrated** — plenty of tutors find inline notes useful, but let anyone who doesn't want them turn them off or push them to the end
- **Student handout** — only ask this if teacher's notes are set to integrated or end-only: whether the tutor also wants a second, notes-free version of the plan suitable for handing to the student. Default: **yes**, unless the tutor opts out

These only need answering once — for a single-lesson request, ask them right away as part of getting into Step 4; for a path, ask once before generating Lesson 1, then reuse the same answers for later lessons unless the tutor wants to change something for a specific one.

Unless the user specifies their own format, include:
1. **Objective(s)** — a can-do statement ("Students will be able to...")
2. **Vocab Review** (2-5 min, only from Lesson 2 of a path onward, or any returning student) — see below
3. **Fresh Vocabulary** — always include this section (see below)
4. **Warm-up** (5-10 min) — activates prior knowledge, low-stakes
5. **Main stage(s)** — presentation/input, controlled practice, freer practice, per the chosen framework
6. **Production/application** — students use the language with some autonomy, built from **several** activities per the chosen selection method (see below), not just one
7. **Wrap-up / assessment** — quick check of the objective (not necessarily formal testing)
8. **Home Assignment** — included by default; omit only if the tutor opted out during the interview
9. **Materials needed**
10. **Timing** for each stage — non-Production stages should sum close to the class length, but the Production stage's activity options can add up to more than what's left (see "Choosing the activities" below); that's intentional headroom, not an error
11. **Teacher's Notes** (if not turned off — see below) — either woven into each stage or collected in one closing section, per the tutor's preference
12. **Student Handout** (included by default whenever Teacher's Notes are on; omit only if the tutor opts out — not possible when Teacher's Notes are off) — a second, separate notes-free copy of the plan (see below)

### Vocab Review section (recycling)
New vocabulary that's never revisited doesn't stick. Before introducing new words, spend a few minutes recycling old ones:
- Pull 2-4 items from the student's Vocabulary Log (in the record file) — prioritize items from lessons that are neither the most recent one (too fresh to need review yet) nor ones already reviewed multiple times, so review rotates through everything taught rather than only ever touching the last lesson's words
- A quick format works fine: a short prompt sentence with the word blanked out, or "use this word in a sentence about..." tied back to the student's interests
- Skip this section for a student's first-ever lesson (nothing to review yet)

### Fresh Vocabulary section
Every lesson gets a standalone new-vocabulary section, not just vocab folded into the presentation stage:
- Use the count the tutor specified (default 6-10 if unspecified), tied to the lesson's theme and therefore to the student's stated interests — every example sentence should read as if written for this specific student, not generic
- **Pitch vocab one CEFR level above the student's current level** (A2 student → A2/B1-boundary or B1 vocab; B1 → B2; B2 → C1; C1 → C2; a C2 student stays at C2 but pushes toward rarer/more nuanced items). This is a deliberate stretch to grow the student, not a comprehension check — use `references/cefr-levels.md` to judge what "one level up" looks like in practice
- Apply the vocab constraint from the interview: if phrasal verbs were requested, every item is a phrasal verb; if a part of speech was requested, every item matches it; otherwise mix naturally around the topic
- Lead with collocations, not isolated definitions (e.g. for "address" as a verb: "address a problem/concern/issue", not just a dictionary gloss) — collocation is what makes vocabulary usable, not just recognizable
- **Never repeat a word/phrase already taught to this student.** Check the student's record file (below) before finalizing the list, and swap out any overlap

**Format** — every vocab item is written exactly as:
```
word – definition
Example: example sentence
```
Use an en dash (–) between word and definition, and the example sentence must connect to the lesson's theme/the student's interests, not be a generic textbook sentence.

### Teacher's Notes
These are for the tutor only — never phrase them as if speaking to the student. Keep each note short (1-2 sentences): pacing/timing guidance, why a stage or activity was chosen, a likely sticking point for this student specifically (e.g. an anticipated L1-interference error, a grammar point they've struggled with before), or a quick alternative if something isn't landing.
- **Integrated** (default): add a brief *Teacher's note:* line (italicized or otherwise visually distinct from the student-facing content) directly under the relevant stage — e.g. a note under Fresh Vocabulary about why this constraint was chosen, or under an activity option about which one to pick if time is short
- **End-only**: skip inline notes entirely and instead add a single "## Teacher's Notes" section as the last part of the plan, with one short bullet per stage that has something worth flagging — don't force a bullet for every stage if there's nothing useful to say
- **Not included**: omit teacher's notes entirely — don't sneak pedagogical asides into stage descriptions instead

### Student handout (optional)
If the tutor opted in, produce a second copy of the plan after the main one, stripped of anything tutor-only:
- Remove all Teacher's Notes, wherever they appear (inline or in the closing section)
- Remove **Materials needed** (that's for the tutor, not the student)
- Keep everything the student would actually see or do: Fresh Vocabulary, Warm-up, Main stage(s), Production activities, Wrap-up, Home Assignment
- Vocab Review is fine to keep — it's a review task for the student, not a tutor-only note
- Label it clearly (e.g. "## Student Handout — <Lesson Name>") so it's obviously the second, separate copy, not a continuation of the main plan

If exporting to `.docx` (Step 6) and a handout was requested, export both as separate files rather than combining them into one document.

### Student Record File (profile, path progress, and vocab log)
Maintain one file per student at `~/eng-teaching-records/<student-name-slug>.md` (lowercase, hyphens for spaces, e.g. `maria-garcia.md`) — **not** inside the skill's own directory (see Step 0 for why). This is the single source of truth that lets a future session say "next lesson for [student name]" without re-running Step 1, and lets vocab stay genuinely new lesson over lesson. It has three parts:

```
# Student Record — <Name>

## Profile
- Pronouns: ...
- Level: ...
- Location: ...
- Interests: ...
- Goal: ...
- Session length: ...
- Activity choice: ...
- Vocab constraint: ...
- Vocab count: ...
- Home assignment: yes/no
- Teacher's notes: integrated/end-only/none
- Key skills weighting: ...
- Exclusions: ...

## Learning Path (omit if single-lesson only)
| Lesson # | Lesson Name | Theme | Grammar Point | Activity Type(s) | Status |
|---|---|---|---|---|---|
| 1 | ... | ... | ... | ... | Delivered 2026-09-24 |
| 2 | ... | ... | ... | ... | Not yet delivered |

## Vocabulary Log
### 2026-09-24 — Lesson 1: <name>
- word one
- word two
```

- **Create/update the Profile** at the end of Step 1 (new student) or whenever the tutor changes something during a returning-student check-in (Step 0)
- **Create/update the Learning Path table** at the end of Step 2, and flip a lesson's Status to "Delivered <date>" immediately after generating it — this is what Step 0 reads to know which lesson is next, without asking the tutor
- **Before** drafting Fresh Vocabulary for any lesson: read the Vocabulary Log section to see everything already taught to this student
- **After** finalizing a lesson's vocab list: append the new items under a new dated heading
- If the tutor mentions the student already knows certain words/phrases (from "anything to incorporate or exclude"), log those too so they're excluded from future lessons even though they weren't taught here

### Choosing the activities
Include **2-3 activities** in the Production/application stage, not a single fixed one — this fills the available time and gives the tutor real options to choose between, skip, or swap live in the lesson rather than being locked into one plan:
- **Random**: pick 2-3 that fit the level, topic, and available time from `references/teacher-activities.md`, varied enough that they're not redundant with each other (e.g. don't pair two debate-style activities back to back) — don't force a 5-item ranking scenario into a 20-minute slot
- **Specific**: use the named activity/activities as given; if the tutor only named one, add 1-2 more (random or judgment-based) so there are still options to fill the time
- **Tutor's judgment / unspecified**: default to `references/teacher-activities.md` first since these are proven-effective for this tutor; fall back to `references/activity-bank.md` only if nothing on the teacher's list fits the lesson's goal

**Keep external-material dependence low.** Most activities generate entirely from text (dialogues, stories, scenarios, discussion questions) and need nothing beyond the plan itself — default to these. Only include **at most 1 activity per lesson** that requires the tutor to source real external material (a found article, a real media clip — the two ⚑-marked entries in `references/teacher-activities.md`), and only when it's clearly the best fit for the goal (e.g. exam prep needing authentic text) or the tutor specifically asked for one. Activities that used to imply a real image or chart (Describe a picture, Gallery tour, data/charts discussion) are generated as vivid written descriptions or text-formatted data by default — see their entries in `references/teacher-activities.md` — so they don't count against this cap unless the tutor wants to supply a real image/chart instead.

Present them as clearly labeled options (e.g. "Activity A / Activity B / Activity C") with a suggested time for each, and note that the tutor can run one, several, or swap based on how the lesson is going and how much time remains. **It's fine — expected, even — for the activities' combined time to run longer than the session length.** These are options to pick from live, not a mandatory sequence, so the tutor isn't meant to run all of them every time. Don't shrink or cut activities just to force the Production stage's total to fit the class length; a plan with more material than one session needs is more useful than one that runs out with time left over.

Calibrate example sentences, instructions, and reading/listening text complexity to the stated CEFR level — don't write B2-complexity examples for an A2 class.

Personalize using what the interview surfaced: swap generic example sentences/topics for ones tied to the student's stated interests, profession, and specific location/hometown (real place names, regional references — not generic "Europe"/"Asia" framing), and pick discussion/reading topics that fit their goal (e.g. workplace scenarios for a business-goal student, campus/exam scenarios for an academic one). Using their location, silently check `references/student-interview.md`'s L1-interference notes and pre-empt the most likely error rather than waiting for it to surface — this stays a background input to your planning, not something stated back to the tutor.

## Step 5: Differentiation and common pitfalls

- If the student's level assessment feels uncertain, or they're clearly having a stronger/weaker day than expected, keep one extension idea and one simplification in your back pocket for the main activity rather than rewriting the whole plan mid-lesson
- Avoid overloading a single lesson with more than one new grammar point or more than ~8-10 new vocab items
- For exam-prep lessons, tie every activity back to the specific exam skill being tested (e.g. IELTS Task 2 essay structure, not just "writing practice")

## Output format

Write the plan as clean markdown with headers per stage and a timing next to each. Don't pad with generic teaching theory the teacher already knows — keep it concrete and usable at the front of a classroom.

## Step 6: Export as a .docx (optional)

A finished lesson plan — or an approved learning path table (see Step 2) — is a document the tutor will print, edit, or hand to a co-teacher, or file away for reference, not something that only lives as chat text. After presenting either one, offer to export it as a `.docx` file the tutor can open in Word/Google Docs and edit further. Don't export automatically — only build the file once the tutor says yes (or asks for a doc/Word file/download up front).

**Default, works on any harness with shell access** (Codex, Cursor, Aider, Claude Code, etc.): write the finished plan to a markdown file, then run the bundled converter:

```bash
python3 skills/eng-teaching/scripts/md_to_docx.py <plan>.md <output>.docx
```

If `python-docx` isn't installed, install it first (`pip install python-docx`) and retry — it's a small, standard library, not a platform-specific plugin. The script (tested end-to-end) handles `#`/`##`/`###` headings, `**bold**`/`*italic*` inline markup, `-`/numbered lists, `> ` blockquote lines (rendered indented, gray, and italic — use this for Teacher's Notes so they read as clearly separate from student-facing content), and standard markdown tables (rendered as a real Word table with a bold header row and gridlines — use this for the learning-path table). Write the source markdown accordingly:
- Lesson title as `#`, each stage (Vocab Review, Fresh Vocabulary, Warm-up, etc.) as `##`
- Bold the vocab word in each `word – definition` line, italicize the example sentence
- Prefix each Teacher's Note line with `> ` so the script styles it distinctly, whether integrated per-stage or collected at the end
- For a learning path, write the path exactly as the `| Lesson # | ... |` markdown table already used to present it — no reformatting needed

**On Claude Code specifically**, the `docx` skill (part of the `document-skills` plugin in the `anthropic-agent-skills` marketplace) is an alternative if it's already installed, and can produce more elaborate formatting via the docx-js API in its `docx-js.md` — but the bundled script above is the default since it needs no extra install and works identically everywhere.

Name the file `<student-name>-lesson-<n>.docx` (or `<student-name>-<lesson-name>.docx` for a single lesson) and save it in the tutor's working directory unless they ask for elsewhere. If a student handout was also requested, name it `<student-name>-lesson-<n>-handout.docx` as a separate file. For a learning path table, name it `<student-name>-learning-path.docx`.
