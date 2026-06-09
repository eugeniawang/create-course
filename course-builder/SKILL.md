---
name: create-course
description: >-
  create-course builds interactive, goal-driven courses delivered in Claude Code,
  Codex, Claude Cowork, or a combination. Turns a topic, goal, existing course, or
  body of source material into a teachable, interactive course: a self-contained
  course folder (README, runtime instructor adapter, full LESSONS, canonical SOURCE,
  progress-tracking templates with active learning time tracking), a personalized
  running-example that every session applies to, scenario checks, explain-back
  moments, remediation/review loops, traceability artifacts, and a browsable
  course landing page that can be deployed live to GitHub Pages. Use when the user
  wants to create/build/generate a
  course, curriculum, lesson plan, training, or its website from some context they
  provide.
---

# create-course

Build interactive, goal-driven courses delivered in Claude Code, Codex, or Claude Cowork.

You build (do not teach) a self-paced, hands-on course in the style of the AIPM /
Dynamic Workflows courses: **an LLM agent is the instructor, learners learn by
DOING, every concept ties to one real thing the learner brings, and each session
ends in a light check.** The end product is a fully self-contained course folder
that the learner can open in Claude Cowork, Codex, or Claude Code and start from
there. You can also generate and deploy a homepage.

create-course is hosted at `eugeniawang/create-course` and is based on the earlier
`eugeniawang/course-builder` fork of John Brophy's `johnbr0phy/course-builder`.
Additional teaching and planning patterns may be
credited when used, including Abiodedeyi's CCA-F reference repo, Carl Vellotti's
`claude-code-pm-course`, `grill-me`'s focused questioning pattern, and structured
planning/validation practices. Optional NotebookLM integration is inspired by
`teng-lin/notebooklm-py`.

This file is the procedure. Three reference docs carry the detail — read the
relevant one before that step:

- `reference/course-planning.md` — planning pass: course identity,
  runtime architecture, learning capabilities, config, validation, roadmap.
  **Read before proposing the outline when the course has meaningful complexity.**
- `reference/notebooklm.md` — optional NotebookLM source and visual-artifact
  workflow. **Read when source-led courses mention NotebookLM, notebooks, visual
  aids, diagrams, infographics, mind maps, or generated study assets.**
- `reference/course-anatomy.md` — exact file structure, lesson format, check rules,
  tracking files, guardrails. **Read before generating course files.**
- `reference/homepage.md` — how to fill and ship the data-driven homepage. **Read
  before generating the homepage.**
- `reference/deploy-pages.md` — the exact GitHub Pages deploy recipe. **Read before
  deploying.**

Templates live in `templates/`; the homepage shell and CI workflow in `assets/`.

---

## Step 1 — Intake (gather context)

Use whatever the user already gave you; only ask for what's missing. Ask briefly,
grouped, and don't interrogate — infer sane defaults and confirm.

Collect:
1. **Build mode** — ask this first:
   - **Greenfield:** "I am starting from scratch with a topic/source/goal."
   - **Brownfield:** "I already have a course in progress and want to reconfigure,
     focus, convert, or improve it."
   For brownfield, collect the existing course folder/files, current runtime, what
   is working, what is broken, and what should remain unchanged. Treat the original
   course as read-only: copy it into a reconstruction workspace inside the new
   self-contained output folder, then rebuild from that copy using these course
   builder guidelines. Preserve useful existing course materials; do not rewrite
   blindly, and never edit the user's original course in place.
2. **Intake template** — classify the request into one of three easy starts:
   - **source-led**: "I have a NotebookLM/notebook, docs, links, files, or source
     packet; build a course around that."
   - **goal-led**: "I need to reach a target: pass an exam, ship a project, do a
     job task, onboard a team."
   - **topic-led**: "I want to learn more about a topic."
3. **Topic / subject** of the course.
4. **Light grill-me goal interview** — every course needs a stated goal, even
   topic-led courses. Ask one focused question at a time, provide a recommended
   default answer, and walk the decision tree only until the course has enough
   shape to avoid sprawl. Resolve: goal type, desired outcome, success criteria,
   audience, delivery environment, source boundary, out-of-scope topics, and capstone.
   Use this decision tree:
   - If source-led: "What should this source help the learner do?" Reject "cover
     everything"; recommend 2-4 outcomes from the source.
   - If goal-led: "What target are we optimizing for?" Tie lessons to the target's
     proof: exam readiness, shipped artifact, job task, or onboarding outcome.
   - If topic-led: "Which practical use of this topic matters most?" Offer
     defaults like explain core concepts, build one artifact, judge good/bad
     examples, or decide whether to use it.
   Stop only when you have: goal type, 2-4 goals, success criteria, out-of-scope
   boundaries, target learner, delivery environment, source boundary, and capstone.
   If any are missing, do not outline yet.
5. **Goal lens** — ask
   whether this is for a specific outcome, general learning/exploration, or both.
   Then force focus: define 2-4 concrete learner goals, what success looks like,
   and what is explicitly out of scope. If the user only says "learn X", guide
   them to choose practical goals before outlining; broad curiosity is not enough.
6. **Source material** — the authoritative content the course must stay accurate
   to. Paste, a file, a URL, or "use your own knowledge of X." This becomes
   `SOURCE.md` and is the source of truth for every lesson and check.
7. **Audience** — who it's for, what they already know, and where they will use it.
8. **Where they PRACTICE** — the hands-on environment every exercise runs in
   (e.g. the Anthropic Console, Claude Code itself, a notebook, a CLI, a sandbox).
   Exercises must be native to this environment.
9. **Delivery environment** — ask where the learner wants to use the finished
   course: Claude Code, Claude Cowork, Codex, or more than one. Default: ask and
   recommend one based on how the learner will use the course. Generate
   `CLAUDE.md` for Claude Code, `COWORK.md` for Claude Cowork, and `AGENTS.md`
   for Codex.
10. **The running example** — the ONE real thing each learner brings that every
   session applies to (a product, a recurring task, a dataset, a repo, a paper…).
   This drives personalization and the capstone. Name the tracking file after it
   (e.g. `MY_PRODUCT.md`, `MY_WORKFLOW.md`, `MY_DATASET.md`).
11. **Size & shape** — number of sessions and phases. Default: derive from the
   source material; if unsure, ~10–12 sessions across 3 phases ending in a
   capstone. Each session ≈ 1 hour.
12. **Learning supports** — whether to enable beginner-first mode, glossary-first
   teaching, competency/domain mapping, source traceability, spaced review,
   scenario gates, practice exams, helper commands, sequential/flexible navigation,
   a reference track, and visual aids.
   Default: enable lightweight versions when the source has certification,
   compliance, professional, or skill-assessment goals.
13. **NotebookLM / visual aids?** — as part of the guided intake, ask:
   - "Do you already have NotebookLM materials for this course?"
   - "Do you want to connect/login now, or leave NotebookLM as optional setup?"
   - "Should the generated course activate visual commands like 'show me a
     diagram', 'make an infographic', and 'make a mind map'?"
   If yes, read `reference/notebooklm.md`, include `NOTEBOOKLM.md`, and add the
   visual commands to the selected runtime adapter(s). Treat `notebooklm-py` as
   optional/unofficial; never require it for the core course.
14. **Course landing page** — generate a quick `index.html` landing page by
    default after the course folder is complete. It must explain what the course
    is, who it is for, what is involved, how to get started, the start command,
    and where the course repo lives. Deploying it live to GitHub Pages is optional
    and should be offered separately.
15. **Self-contained output folder** — confirm the generated/reconfigured course
    folder will contain everything needed to start learning: runtime adapter(s),
   lessons, source, learner templates, progress and active-time tracking, optional
   materials, brownfield reconstruction copy if applicable, and any setup notes. No
   hidden dependency on the builder repo, and no mutation of original source inputs.
16. **Any guardrails** specific to the domain (cost, safety, tool limits) to bake
   into the instructor.

## Step 2 — Plan the course architecture, then confirm

For small courses, draft the outline directly. For certification, professional,
multi-runtime, or source-heavy courses, read `reference/course-planning.md` and
produce a short `COURSE_PLAN.md` first. Keep it practical:

- course identity and one-line promise
- goal lens, success criteria, and out-of-scope boundaries
- delivery environment and adapter decision
- optional NotebookLM/materials workspace and visual artifact policy
- source/traceability policy
- learner capabilities and assessment gates
- configuration variables and external dependencies
- build roadmap and validation checklist

Then draft the session list: title + one-line description per session, grouped
into phases, with the final session as a capstone that ships the learner's
running example. State the running example, delivery environment, and where
exercises will be practiced. Get a quick thumbs-up (and let them adjust) **before** writing
full content.

## Step 3 — Generate the course folder

Read `reference/course-anatomy.md`. Create a self-contained `<slug>-course/`
folder at the repo root (don't modify unrelated files). Produce, as **real
content, not stubs**:

- `SOURCE.md` — the canonical source material, faithfully.
- Lessons, in full (no stubs), each with: **Overview · Skill built ·
  Prerequisites/foundations · Plain-language concept · Do this first · What just
  happened · Guided practice · Explain it back · Pattern/anti-pattern · Scenario
  check · Apply/transfer · Review hooks**. **Split for token economy:** for larger
  courses, put one tightly-scoped session per file `lessons/<NN>-<slug>.md` and make
  `LESSONS.md` a **lean index** (TOC + cross-cutting header: lesson format,
  guardrails, principle spine) — the runtime loads only the current session file.
  Tiny courses (≤~4 sessions) may keep full content inline in a single `LESSONS.md`.
- `reference/` (optional but recommended) — static study aids built ONCE at build
  time: a self-contained, printable `reference-card.html` (start from
  `templates/reference-card.html`) and an optional client-side `quiz.html`. No
  external/CDN deps, no model needed at use; opened in a browser, never regenerated
  per turn. Lessons are the teaching; reference is the durable revisited artifact.
- `CLAUDE.md`, `COWORK.md`, and/or `AGENTS.md` — runtime adapter(s): setup flow,
  how to run a session, navigation mode, commands, check rules, helper commands,
  remediation rules, the personalization rule, and domain guardrails.
- `README.md` — overview, quick start, recommended sequential path with flexible
  navigation option, the session list, prerequisites.
- `index.html` — a quick course landing page: what the course is, what is
  involved, how to get started, and where the repo lives.
- `COURSE_PLAN.md` — optional planning/validation artifact for complex courses.
- `VALIDATION.md` — required verification record with structural checks,
  harness-specific smoke proof, self-contained folder audit, adversarial coherence
  review, coverage gaps, and fixes applied.
- `templates/` — `user.json`, `progress.json`, `PROGRESS.md`, and the running-
  example file (`MY_*.md`), plus optional `GLOSSARY.md` and `COMPETENCY_MAP.md`
  when useful. For NotebookLM-backed courses, include `NOTEBOOKLM.md`.

Adapt the templates in this skill's `templates/` folder; fill every placeholder.
Keep explanations short and practical — "why this matters" over theory.

## Step 4 — Generate the course landing page

Read `reference/homepage.md`. Copy `assets/index.html` into the course folder and
fill its `SITE` config + `phases` data from `LESSONS.md`. It is the course's quick
GitHub Pages landing page, modeled on `https://johnbr0phy.github.io/course-builder/`:
what this course is, what is involved, how to start, and the repo link. It's a
single self-contained file (React + Tailwind via CDN, no build step). Validate the
JSX transpiles before finishing. Only skip it if the user explicitly says they do
not want a landing page.

## Step 5 — Deploy live (if wanted)

Read `reference/deploy-pages.md` and follow it exactly. Copy
`assets/deploy-pages.yml` to `.github/workflows/`, point it at the course folder
and the current branch, push to trigger it, confirm the run is green, and give the
user the live URL. If Pages isn't enabled yet, tell the user the one setting to
flip (Settings → Pages → Source → **GitHub Actions**) and re-trigger with a push.

Before deploying, do a privacy check. Do not publish `SOURCE.md`, `user.json`,
`progress.json`, `PROGRESS.md`, `materials/`, NotebookLM exports, customer/source
docs, or brownfield copies unless the user explicitly wants those files public.
Prefer deploying a dedicated public folder that contains only `index.html` and
approved public assets.

## Step 6 — Verify the generated course

Before finishing, run a clean final verification pass against the **actual
generated course folder**, not just the outline or diff. This is mandatory because
the finished course must be internally coherent, self-contained, and runnable in
the selected delivery harness.

Write the results into `VALIDATION.md`, then fix clear issues before the final
response. Do not mark the course done with unresolved blockers unless you name the
blocker and the exact next action.

Verification has four parts:

1. **Structural validation**
   - Required files exist for the selected runtime(s).
   - JSON files parse.
   - Session counts match across `README.md`, `LESSONS.md`, `PROGRESS.md`, and
     `progress.json`.
   - No unfilled placeholders remain.
   - `README.md` quick start matches the generated runtime adapter commands.
   - Lessons include guided practice, explain-back, scenario check, apply/transfer,
     and review hooks.
   - For per-session split courses, each `lessons/<NN>-…` file is complete and loads
     individually; `LESSONS.md` is a lean index and the adapter loads only the
     current session file.
   - Any `reference/` study aids are self-contained HTML (no external/CDN deps) and
     work with no model at use; they are not regenerated per turn.
   - A model-guidance note is present (recommended teaching tier; reference aids need
     no model; not over-spec'd).
   - Any course-shipped skills/commands are lean: progressive-disclosure `SKILL.md`,
     minimal `allowed-tools`, thin command wrappers, no cross-skill duplication.
   - Source credit, source anchors, and validation notes are present where needed.

2. **Self-contained folder audit**
   - The learner can start from only the generated folder.
   - No required step depends on this builder repo, the original brownfield folder,
     a private path, or an unstated external file.
   - Setup copies every required template into root working files.
   - Practice data, setup notes, and optional external integrations are either
     included or clearly marked optional with fallback behavior.

3. **Harness-specific smoke proof**
   - Confirm the selected delivery harness: Claude Code, Claude Cowork, Codex, or
     a selected combination.
   - Verify the correct adapter exists: `CLAUDE.md`, `COWORK.md`, and/or
     `AGENTS.md`.
   - Check that expected commands are defined for that harness: `start`,
     `continue`, `help`, `define X`, progress, review, and session navigation.
   - Check structured-input/tool assumptions. If a harness lacks an expected tool,
     document the plain-chat fallback in the adapter and in `VALIDATION.md`.
   - Dry-run the first-run setup path and one start/resume path by reading the
     adapter against the generated files. When live runtime execution is possible,
     run it; otherwise record this as structural proof only and say runtime proof
     is still pending.

4. **Adversarial coherence review**
   - Review the course as a skeptical learner and maintainer.
   - Look for contradictions, missing prerequisites, undefined jargon, broken
     source traceability, impossible exercises, dead commands, weak remediation,
     non-self-contained dependencies, and mismatches between adapters.
   - Record each finding in `VALIDATION.md` as fixed, accepted with rationale, or
     blocked with next action.

## Step 7 — Finish

Print the final file tree and the **exact first command the learner types to
start** (e.g. `"Let's get started"`). If you deployed, give the live URL.

**Do not start teaching. Build the course, then stop.**

---

## House rules (John's preferences — apply to EVERY course built)

These override anything else in this skill or its references:

1. **Style**: concise and fun, with a British sense of humour — wry, understated,
   happy to take the mickey out of things that go wrong. No earnest corporate tone.
2. **Format**: short bits of text only. A few lines per instructor turn, max.
   Never walls of text, never long concept explanations up front. One idea at a time.
3. **Doing before telling**: every lesson STARTS with the learner doing something
   real and seeing the result. The result teaches the concept; the explanation
   comes after, in two or three lines. Never explain a concept then check it.
4. **Real tools, real data**: include login links to the actual tools used. Ship
   test data with the course so the learner can run things immediately and see
   real output — not hypotheticals or toy descriptions of what would happen.
5. **Checks**: rare and light. Prefer "predict what happens, then run it and
   check", scenario judgment, and "explain it back" over multiple-choice recall.
6. **Never show code.** The learner is non-technical. Describe what things do in
   plain English.
7. **Copy-paste-ready text**: anything the learner needs to paste goes in a fenced
   code block (copy button), personalized to their project. Never blockquotes.
8. **Structured input when available**: run interviews and checks through the
   runtime's structured user-input tool rather than open chat questions.
9. **Budget checks**: where the domain has costs/credits, have the learner paste
   the live number from the tool and diff against baseline.

## Principles (carry through every step)

- **Build, don't teach.** This skill produces the course; the generated runtime
  adapter (`CLAUDE.md`, `COWORK.md`, and/or `AGENTS.md`) is what teaches later.
- **Learn by doing, in their environment.** Every exercise is hands-on and native
  to the practice environment from intake.
- **One running example, everywhere.** Personalization is the spine: capture the
  learner's real thing up front and apply each session's concept to it; the
  capstone ships it.
- **Self-contained & non-destructive.** Everything lives in the new course folder
  (plus an optional workflow under `.github/`). Don't touch unrelated files.
- **Faithful to source.** `SOURCE.md` wins any disagreement; lessons and checks
  must stay accurate to it.
- **Beginner-first when configured.** Define jargon on first use, repair missing
  foundations before advancing, and keep weak concepts recurring in later review.
- **Traceable when needed.** Competencies, scenarios, and checks should point back
  to source sections or validation notes when the course has assessment stakes.
- **Progressive disclosure / token economy.** Don't pull the whole lesson corpus
  into context to teach one session. Split larger courses into per-session
  `lessons/<NN>-…` files with a lean `LESSONS.md` index; the runtime loads only the
  current session file. State the rationale; keep it proportionate (tiny courses may
  stay inline).
- **Static aids are prebuilt, never per-turn.** `reference/` study aids are
  self-contained HTML built once at build time and opened in a browser with no model
  at use. Interactivity in the live course comes from the runtime conversation;
  reserve static HTML for durable/interactive artifacts, not per-turn generation
  (token-expensive and weakens the conversation).
- **Model guidance.** Every generated course states what model to run on: a
  recommended tier for the judgment-heavy teaching, a note that `reference/` aids
  need no model, and no over-speccing (Opus is rarely needed). Surface it in the
  plan, the adapter(s), and the homepage/README.
- **Lean skill conventions** (when a course ships its own skills/commands, per
  Anthropic Agent Skills best practices): progressive-disclosure `SKILL.md` that
  points to canonical files rather than restating them; **minimal `allowed-tools`**
  (only what's used); thin command wrappers that just route to the skill; no
  cross-skill duplication; never load a whole large file when one section/file does.
