# Course Anatomy

The exact structure, formats, and rules every generated course follows. This is
the distilled blueprint behind the AIPM and Dynamic Workflows courses. The output
must be a self-contained folder: a learner can open that folder in Claude Cowork,
Codex, or Claude Code and start the course without needing this builder repo.

## File structure

```
<slug>-course/
├── README.md          # overview, quick start, session list, prerequisites
├── CLAUDE.md          # Claude Code instructor adapter, when selected
├── COWORK.md          # Claude Cowork instructor adapter, when selected
├── AGENTS.md          # Codex instructor adapter, when selected
├── LESSONS.md         # lean index for larger courses (TOC + cross-cutting header);
│                      # OR full plans inline for tiny courses (≤~4 sessions)
├── lessons/           # one tightly-scoped session per file, loaded individually
│   ├── 01-<slug>.md   # full plan for session 1 (real content, not stubs)
│   ├── 02-<slug>.md
│   └── …
├── reference/         # static study aids — prebuilt, self-contained, model-free
│   ├── reference-card.html  # printable cheat-sheet, opened in a browser
│   └── quiz.html            # optional client-side interactive quiz/widget
├── SOURCE.md          # canonical source material — lessons stay accurate to this
├── COURSE_PLAN.md     # optional planning/validation artifact for complex courses
├── NOTEBOOKLM.md      # optional NotebookLM/materials/visual artifact plan
├── VALIDATION.md      # required structural/runtime/adversarial proof record
├── index.html         # GitHub Pages-ready course landing page, self-contained
└── templates/
    ├── user.json          # learner profile (copied to course root on setup)
    ├── progress.json      # navigation, status, time tracking, checkpoints
    ├── PROGRESS.md        # human-readable checkmark + active-time view
    ├── GLOSSARY.md        # optional first-use definitions
    ├── COMPETENCY_MAP.md  # optional domains, scenarios, source traceability
    ├── NOTEBOOKLM.md      # optional NotebookLM setup + artifact log
    ├── VALIDATION.md      # required maintainer validation record
    └── MY_<THING>.md      # the learner's running example → capstone artifact
```

On first run the instructor copies the templates into working files at the course
root (`user.json`, `progress.json`, `PROGRESS.md`, `MY_<THING>.md`, plus optional
`GLOSSARY.md` and `COMPETENCY_MAP.md`). `VALIDATION.md` is generated at build
time and completed by the builder before finish.

Greenfield courses create this folder from scratch. Brownfield courses reconfigure
an existing folder into this shape. Treat the original course as read-only: copy it
into a reconstruction workspace inside the new self-contained folder, rebuild from
that copy, and preserve useful lessons, sources, assets, and learner-facing
conventions unless the user asks to replace them.

`<slug>` is a kebab-case name from the topic (e.g. `dynamic-workflows-course`).
`MY_<THING>` is named after the running example (`MY_PRODUCT`, `MY_WORKFLOW`,
`MY_DATASET`, `MY_PAPER`, …).

## SOURCE.md

The authoritative material, reproduced faithfully (verbatim if the user supplied
it). Open with a short note: *"This is the source of truth. If a lesson and this
file disagree, this file wins."* Every lesson and check answer must trace back here.

## Progressive disclosure (token economy)

Lessons are the bulk of a course's words. Loading the entire corpus to teach one
session is wasteful and degrades the conversation — don't pull 15k+ words into
context to run a single 1-hour session.

- **Larger courses (the recommended default):** keep each session in its own file,
  `lessons/<NN>-<slug>.md` (one tightly-scoped session each). `LESSONS.md` becomes a
  **lean index** — a table of contents plus the cross-cutting header (lesson format,
  guardrails, any principle spine). The runtime loads **only the current session
  file**, never the whole corpus.
- **Tiny courses (≤~4 sessions):** a single `LESSONS.md` with full content inline is
  fine; the split would be over-engineering. Keep it proportionate.

Whichever shape, state the rationale in the adapter so the instructor never reads
more than the one session it's teaching.

## Static study aids (`reference/`)

Pre-built, **self-contained HTML** artifacts that need **no model at use** — built
ONCE at course-build time, shipped in the folder, and opened in a browser. Examples:
a printable reference/cheat-sheet card and an optional interactive client-side
quiz/widget (all logic inline, no external/CDN deps). They are **never regenerated
per turn**: per-turn HTML is token-expensive and weakens the conversation.

The distinction (credit Matt Pocock's `teach` skill — "lessons are rarely revisited;
reference docs are"):

- **Lessons = the teaching.** Conversational, personalized, run live in the runtime.
- **Reference = the durable, revisited, model-free artifact.** A learner returns to
  the cheat-sheet long after the lesson conversation is gone.

The runtime gets a command to open the aids; it does not rebuild them.

## LESSONS.md — the lesson format

Whether inline (tiny course) or per-file under `lessons/` (larger course): top of
the index/file names the lesson format and lists the cross-cutting guardrails the
instructor enforces in every exercise. Then a section per phase, a subsection (or
file) per session. **Every session includes these parts:**

1. **Overview** — one sentence naming the goal.
2. **Skill built** — the concrete capability the learner practices.
3. **Prerequisites / foundations** — terms or ideas to repair before advancing.
4. **Do This First** — the session OPENS with the learner doing something real and
   seeing the result. Concrete, runnable, on real tools with shipped test data.
   Include login links. Add any safety/cost warning the domain needs.
5. **What Just Happened** — two or three lines explaining the result they just
   saw in simple language. Define jargon on first use. Never a wall of text,
   never theory before the doing.
6. **Guided Practice** — one or two scaffolded exercises native to the practice
   environment.
7. **Explain It Back** — the learner explains the idea simply; the instructor
   repairs gaps before moving on.
8. **Pattern / Anti-pattern** — the good pattern, the common failure mode, and
   how to decide between them.
9. **Scenario Check** — a realistic judgment check mapped to a competency when
   `COMPETENCY_MAP.md` exists.
10. **Apply / Transfer** — update the relevant line in `MY_<THING>.md`, tying the
   session's concept to the learner's running example.
11. **Review Hooks** — weak concepts to revisit later and cumulative review
   prompts for session, week, or phase gates.

Keep it practical and short. Concepts ground out in the running example and the
source material, never abstract theory for its own sake.

## Runtime adapter — `CLAUDE.md`, `COWORK.md`, `AGENTS.md`, or selected combination

Ask the course creator which delivery environment the learner wants to use:
Claude Code, Claude Cowork, Codex, or more than one. Generate `CLAUDE.md` for
Claude Code, `COWORK.md` for Claude Cowork, and `AGENTS.md` for Codex. The
teaching behavior must stay equivalent across adapters; only runtime-specific
wording/tool references should differ.

Each adapter must contain these sections:

- **First-run setup flow** (when `user.json` is missing): ask, one question at a
  time, for the learner's name, role, relevant background, and — most important —
  a rich capture of their **running example** (a concrete recent instance, its
  inputs, its output, where it's slow/weak today) plus a 1–5 self-rating that
  calibrates depth. Then create `user.json` and copy the selected templates to the
  course root. Play the captured example back and confirm before starting.
- **Session execution sequence**: load `user.json` → **load only the current
  session file `lessons/<NN>-…`** (use the `LESSONS.md` index only to find which
  file; for a tiny inline course, read just that session's section) → check
  `progress.json` → state the objective in one sentence →
  guide exercises step-by-step, **one step then wait for the learner to paste what
  they saw** → discuss → do "Apply to Your Work" → run the scenario check → update
  `progress.json` and `PROGRESS.md`.
- **Recognized commands**: "Start", "Let's do Session X", "Jump to Session X",
  "Use sequential mode", "Use flexible mode", "Continue my course", "Resume",
  "Help", "Define X", "Quiz me", "Practice exam", "Show my progress", "What's
  next?", "I'm stuck on X", "Parking lot".
- **Navigation mode**: recommend sequential learning because lessons scaffold on
  each other. If the learner chooses flexible mode, allow jumping around, warn
  about missing prerequisites, offer a small bridge, log the jump, and keep
  `current_session` as the best recommended sequential next step.
- **Focus and side-question rules**: answer blockers briefly, park interesting
  side questions, convert misconceptions into remediation, and only advance the
  session after the session check.
- **Scenario/check rules** (see below).
- **Personalize every exercise** rule: once `user.json` exists, rewrite each
  exercise around the learner's real running example; the generic prompts in
  `LESSONS.md` are fallbacks only.
- **Open the reference aids**: a command (e.g. "Open the reference card") that
  points the learner at the prebuilt files under `reference/` — opened in a browser,
  never regenerated per turn.
- **Model guidance**: a short note on what model to run on — recommend a tier for
  the judgment-heavy teaching, note the static `reference/` aids need no model at
  use, and don't over-spec (Opus is rarely needed).
- **Guardrails**: any domain-specific cost/safety rules to enforce in exercises.
- **Teaching philosophy**: hands-on over lecture; proceed deliberately; stay
  concise; ground everything in the running example; surface tradeoffs; knowing
  when NOT to use a thing is a first-class goal.
- **Delivery style**: concise and fun with a wry, understated British sense of
  humour — take the mickey out of things that go wrong, never earnest corporate
  tone. A few lines per turn, max; one idea at a time. Doing before telling,
  always. Never show code — describe what things do in plain English. Anything
  paste-able goes in a fenced code block, personalized. Use structured user-input
  tools for interviews and checks when available.

## Scenario/check rules (copy into every runtime adapter)

Checks are rare and light — not an exam at the end of every session:
- Prefer **"predict, then run, then compare"**: the learner says what they expect,
  runs the real thing, and reconciles the difference. The gap is the lesson.
- Prefer realistic scenario and judgment questions over recall-only checks.
- Draw from the session's **Scenario Check** and adapt freely to the learner's
  running example.
- If structured user-input tools are available, use them for the predict step.
- Note outcome, weak concept, misconception, retry status, and recommended next
  action in `progress.json` and `PROGRESS.md`.
- Use targeted retry for gates: explain-again, guided-retry, or advance with a
  scheduled review.

## Tracking files

- **user.json** — name, role, background, the running-example capture, comfort
  rating, beginner-first preference, created date. Name the personalization fields
  after the running example.
- **progress.json** — one entry per session with `status`
  (`not_started`/`in_progress`/`completed`), `active_minutes`, `started_at`,
  `completed_at`, `time_log`, `checkpoints`, `check_result`, `weak_concepts`,
  `misconceptions`, `retry_status`, `recommended_next_action`, and `notes`, plus
  top-level `course_meta`, `current_session`, `navigation`, `time_tracking`,
  `review_queue`, `parking_lot`, and `competency_progress`. `course_meta` stores
  the goal lens, success criteria, out-of-scope boundaries, delivery environment, source
  boundary, and capstone so "continue" can keep the course aligned. `navigation`
  stores sequential/flexible mode and jump history. `time_tracking` stores active
  learning time only, with pause/resume support for breaks and side quests.
- **PROGRESS.md** — checkmark view grouped by phase, with active learning time,
  checks, weak areas, review queue, and next action.
- **README.md** — includes a required Course focus section: goal, success
  criteria, out-of-scope, and capstone/proof. This prevents simple courses from
  skipping the anti-sprawl scaffold.
- **MY_<THING>.md** — starts as the learner's running example; gains a line per
  session showing how that session's concept applies; ends as the capstone artifact.
- **GLOSSARY.md** — optional plain-language definitions and first-use locations.
- **COMPETENCY_MAP.md** — optional domains, scenario IDs, source anchors, and
  remediation paths.
- **COURSE_PLAN.md** — optional planning artifact: identity,
  runtime architecture, capabilities, config, dependencies, validation, roadmap.
- **NOTEBOOKLM.md** — optional NotebookLM-backed materials workspace and visual
  artifact log for diagrams, infographics, mind maps, and study assets.
- **VALIDATION.md** — required maintainer record separating structural validation,
  self-contained folder audit, harness-specific smoke proof, adversarial coherence
  review, coverage gaps, and unresolved runtime-proof limits.

## Personalization (the spine)

The single most important quality bar: the course must make sense **using the
learner's own example**. Capture it richly at setup, store it in `user.json` and
`MY_<THING>.md`, and have the instructor run every exercise on it. A course whose
exercises are generic toys is a failure of this skill.

## Quality bar / definition of done

- Self-contained folder; nothing unrelated modified.
- Brownfield rebuilds never edit the original course; they copy it first, preserve
  useful existing content, and document what changed.
- Every session has **full** content (no "TODO"/stubs), whether inline in
  `LESSONS.md` (tiny course) or in per-session `lessons/<NN>-…` files (larger
  course). For the per-file split, `LESSONS.md` is a lean index and the runtime
  loads only the current session file — never the whole corpus.
- Static `reference/` study aids (cheat-sheet card, optional quiz/widget) are
  self-contained HTML with no external/CDN deps, work with **no model at use**, and
  are built once at build time — never regenerated per turn.
- A short **model-guidance** note is present (recommended tier for teaching;
  reference aids need no model; don't over-spec).
- Any skills/commands the course ships are lean: progressive-disclosure `SKILL.md`
  (points to canonical files, doesn't restate), **minimal `allowed-tools`**, thin
  command wrappers that route to the skill, no cross-skill duplication.
- `index.html` is present unless explicitly declined, and clearly explains what
  the course is, what is involved, how to start, and where the repo lives.
- Final session is a **capstone** that ships the learner's running example.
- All selected templates present and internally consistent (session count matches
  across `progress.json`, `PROGRESS.md`, `LESSONS.md`, `README.md`).
- Active learning time is tracked in `progress.json.time_tracking` and summarized
  in `PROGRESS.md`.
- JSON validates.
- Delivery environment choice is honored: `CLAUDE.md`, `COWORK.md`, `AGENTS.md`, or a
  selected combination. If multiple, behavior matches across adapters.
- Focus guardrails exist: side questions are answered, parked, or converted into
  remediation without losing the current session objective.
- Source credit and traceability are preserved when inspiration/source materials
  shape the course.
- Complex courses include a `COURSE_PLAN.md` or equivalent notes so architecture
  decisions and validation criteria are recoverable.
- Every course includes `VALIDATION.md` or equivalent notes for structural checks,
  self-contained folder audit, harness-specific smoke proof, adversarial coherence
  review, coverage gaps, and maintainer notes.
- The final builder step has run against the actual generated folder, with clear
  issues fixed before handoff and runtime-proof limits recorded.
