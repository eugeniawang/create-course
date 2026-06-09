# Course Planning

Use this planning pass when a course has certification stakes, multiple delivery
environments, source traceability, external dependencies, or enough scope that a quick
outline would hide important decisions.

## When To Use

Use this planning pass by default for:
- certification or competency-based courses
- beginner-first courses with remediation paths
- courses delivered in Claude Code, Codex, Claude Cowork, or more than one
- courses with source maps, scenario maps, or validation artifacts
- courses with external tools, credits, setup, or safety constraints

Skip it for tiny one-off courses where a direct outline is enough.

## Planning Phases

1. **Build Mode** — greenfield from scratch or brownfield reconfiguration of an
   existing course. Brownfield must identify what to preserve, replace, and
   migrate into the self-contained folder shape. Never edit the original course:
   copy it into the new output folder as the reconstruction source, then rebuild
   from that copy.
2. **Identity** — course title, slug, audience, promise, source inspiration, and
   what the learner can do at the end.
3. **Light Grill-Me Goal Interview** — one focused question at a time, with a
   recommended default answer. Resolve goal, outcome, audience, delivery environment,
   source boundary, scope, and capstone before outlining.
4. **Goal Lens** — specific outcome vs general learning; 2-4 course goals;
   success criteria; out-of-scope boundaries. This prevents course sprawl.
5. **Runtime Architecture** — Claude Code, Claude Cowork, Codex, or combinations; adapter files to
   generate; runtime-specific tool affordances; start/resume commands.
6. **Learning Architecture** — phases, sessions, capstone, prerequisites,
   glossary-first mode, Feynman explain-back, scenarios, review gates.
   Decide here:
   - **Progressive disclosure** — for larger courses, plan per-session
     `lessons/<NN>-…` files with a lean `LESSONS.md` index so the runtime loads only
     the current session (don't pull the whole corpus per session). Tiny courses
     (≤~4 sessions) may keep `LESSONS.md` inline.
   - **Static study aids** — which prebuilt, self-contained, model-free `reference/`
     artifacts to ship (e.g. printable cheat-sheet card, optional client-side quiz).
     Built once at build time, opened in a browser, never regenerated per turn.
   - **Model guidance** — what model to run on: a recommended tier for the
     judgment-heavy teaching, a note that `reference/` aids need no model, and no
     over-speccing (Opus rarely needed). Carry it into the adapter and homepage.
7. **Capabilities** — learner-facing commands and instructor behaviors. Treat each
   command as a capability with inputs, outputs, and success criteria.
8. **Configuration** — setup questions, defaults, learner profile fields,
   progress fields, and optional files to copy.
9. **Dependencies and Guardrails** — practice environment, login/setup, metered
   credits, safety rules, source freshness, validation limits.
10. **Validation and Roadmap** — what must be true before the course is done, what
   gets built first, and what can be deferred.

## Quick Intake: Goal Lens

Before outlining, ask enough to turn a topic into a focused course:

Use a light version of the `grill-me` repo's questioning pattern: ask one
question at a time, recommend a default, and stop once the course has enough
parameters. This is not a stress test; it is a short focusing interview.

| Intake template | Use when | Required focus question |
| --- | --- | --- |
| Source-led | User has NotebookLM/notebook, docs, links, files, or a source packet. | "What should this source help the learner do?" |
| Goal-led | User has an exam, certification, work task, project, onboarding, or deadline. | "What target are we optimizing for?" |
| Topic-led | User wants to learn a general topic. | "Which 2-4 practical learning goals matter most?" |

| Question | Why it matters |
| --- | --- |
| Are we building from scratch or reconfiguring an existing course? | Determines greenfield vs brownfield workflow. Brownfield originals are read-only and copied before reconstruction. |
| Is this for a specific goal, general learning, or both? | Sets assessment depth and scope. |
| What should the learner be able to do at the end? | Converts curiosity into outcomes. |
| What would count as success? | Defines final checks and capstone. |
| What is out of scope? | Prevents sprawl. |
| What deadline or external standard exists? | Shapes pacing and gates. |
| Which delivery environment should teach it: Claude Code, Claude Cowork, Codex, or more than one? | Determines adapter files and interaction style. |
| Do you have NotebookLM materials, and should we connect/login now? | Enables source-led visuals without forcing the dependency. |
| Should visual commands be active in the course? | Keeps diagrams/infographics intentional, not decoration. |

If the answer is "I just want to learn more about X", treat it as topic-led and
help the user choose 2-4 concrete learning goals, such as "explain the core
concepts", "make one working artifact", "judge good vs bad examples", or
"decide whether to use this in my work." Do not build an encyclopedic course.

## Capability Review

Before generating full lessons, review the capability list:

| Capability | Input | Output | Success criteria |
| --- | --- | --- | --- |
| start / continue | learner state | next session action | learner knows exactly what to do |
| define | term | plain-language definition | learner can use the term correctly |
| quiz-me | weak/current concepts | scenario checks | misconception logged or cleared |
| practice-exam | competency map | cumulative scenarios | readiness signal + remediation |
| progress | progress files | next action | weak areas and review queue visible |
| show diagram / infographic / mind map | concept + source anchor | visual artifact or fallback explanation | learner sees the relationship/process clearly |

Add or remove rows based on the course. Do not cargo-cult commands that do not
serve the learner.

## Validation Checklist

- Delivery environment honored: `CLAUDE.md`, `COWORK.md`, `AGENTS.md`, or selected combination.
- Goal lens is explicit: outcome/general learning, goals, success criteria, and
  out-of-scope boundaries.
- NotebookLM decision is explicit: disabled, optional setup, or connected during
  intake; visual commands match that choice.
- Session count matches `README.md`, `LESSONS.md`, `PROGRESS.md`, and
  `progress.json`.
- Lessons include overview, skill built, prerequisite repair, guided practice,
  explain-back, scenario check, application, and review hooks.
- Progressive disclosure honored: larger courses split into per-session
  `lessons/<NN>-…` files with a lean `LESSONS.md` index; the runtime loads only the
  current session file (tiny courses may stay inline).
- Static study aids in `reference/` are self-contained HTML (no external/CDN deps),
  work with no model at use, and are built once — never regenerated per turn.
- Model guidance is explicit: recommended teaching tier, reference aids need no
  model, not over-spec'd.
- Glossary terms are defined before first dependency.
- Scenario checks map to competencies/source anchors when relevant.
- Weak concepts have remediation and spaced-review paths.
- NotebookLM/visual commands are included only when useful, optional, and backed
  by source notes.
- Source credit and validation notes are preserved.
- JSON validates.
