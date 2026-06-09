# create-course

Build interactive, goal-driven courses delivered in Claude Code, Codex, or Claude Cowork.

create-course helps you make a self-contained course folder from a topic, source
packet, goal, or existing course. The finished course can be opened in Claude
Code, Codex, or Claude Cowork. Those are delivery environments, not the subject
of the course.

It follows a simple pattern: the course teaches by doing, uses the learner's own
example, checks understanding with light scenario questions, and tracks progress
as the learner goes. Give it a topic, goal, source packet, or existing course,
and it generates a starter course folder you can review and refine.

Landing page: https://eugeniawang.github.io/create-course/

---

## What it generates

```
<topic>-course/
├── README.md          overview & quick start
├── CLAUDE.md          Claude Code course instructions, when selected
├── COWORK.md          Claude Cowork course instructions, when selected
├── AGENTS.md          Codex course instructions, when selected
├── LESSONS.md         full session plans (real content, not stubs)
├── SOURCE.md          source material the lessons are based on
├── COURSE_PLAN.md     optional planning/validation artifact for complex courses
├── NOTEBOOKLM.md      optional NotebookLM/materials/visual artifact plan
├── VALIDATION.md      optional validation notes
├── index.html         GitHub Pages-ready course landing page
└── templates/
    ├── user.json
    ├── progress.json
    ├── PROGRESS.md
    ├── GLOSSARY.md / COMPETENCY_MAP.md / COURSE_PLAN.md / NOTEBOOKLM.md / VALIDATION.md
    └── MY_THING.md    the learner's running example → capstone
```

- **An instructor, not a document** — `CLAUDE.md`, `COWORK.md`, and/or `AGENTS.md`
  runs sessions, waits for the learner, and checks understanding.
- **Delivery environment choice** — create course instructions for Claude Code,
  Codex, or Claude Cowork.
- **Personalized throughout** — every exercise runs on the one real example the
  learner brings; the final session turns it into a finished project.
- **Hands-on by design** — learning happens in the real environment.
- **Feynman explain-back** — the course asks learners to explain concepts in
  simple language and repair gaps.
- **Scenario checks** — checks favor judgment and application over recall-only
  questions.
- **Sequential or flexible navigation** — sequential is recommended, but learners
  can jump around with prerequisite warnings.
- **Remediation and spaced review** — weak concepts recur through explain-again,
  guided retry, and review queues.
- **Progress and active time** — generated courses track completion, weak areas,
  retries, next action, and active learning time.
- **Safe for existing courses** — existing courses are copied into the generated
  folder first; originals stay read-only.
- **Source notes** — optional maps, source anchors, validation notes, and
  coverage artifacts keep lessons grounded.
- **Optional visual aids** — source-led courses can use NotebookLM-backed
  materials to generate diagrams, infographics, mind maps, and study assets.
- **Course landing page** — a simple page explaining what the course is, how to
  start, and where the repo lives.

## Install

create-course is not an npm package yet. For now, install it by copying the
`course-builder/` folder into the project where you want to create a course.

For Claude Code, open your project and paste:

```text
Install create-course in this project. Clone https://github.com/eugeniawang/create-course, copy its course-builder/ folder into .claude/skills/course-builder/, then delete the clone. Tell me when SKILL.md is in place.
```

Or run this from the project root:

```bash
git clone --depth 1 https://github.com/eugeniawang/create-course.git .cf-tmp
mkdir -p .claude/skills
cp -r .cf-tmp/course-builder .claude/skills/course-builder
rm -rf .cf-tmp
```

For Codex, open your project and paste:

```text
Install create-course in this project. Clone https://github.com/eugeniawang/create-course, copy its course-builder/ folder into .courseforge/course-builder/, then add a short AGENTS.md note that says: "When I ask you to build a course, follow .courseforge/course-builder/SKILL.md." Tell me when that is in place.
```

For Claude Cowork, open the project and paste:

```text
Install create-course in this project. Clone https://github.com/eugeniawang/create-course, copy its course-builder/ folder into a project-local create-course folder, then add a project instruction telling Claude Cowork to follow course-builder/SKILL.md when I ask it to build a course. Tell me where you put SKILL.md.
```

## Use

In Claude Code or Codex, ask the installed builder:

```
Build a course on <your topic> for <your audience>.
```

The builder runs a short goal-focusing intake, asks where the learner will open
the finished course, proposes an outline, you confirm, and it builds the course.
It stops there; the generated course is what teaches.

## Publish the repo landing page

This repo includes a public landing page at `docs/index.html`.

To make it visible on GitHub:

1. Commit and push this repo to GitHub.
2. Open the repo on GitHub.
3. Go to **Settings -> Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**.
5. Select branch `main` and folder `/docs`.
6. Save.

For this repo, GitHub publishes the page at:

```text
https://eugeniawang.github.io/create-course/
```

For forks, the URL is usually `https://<owner>.github.io/<repo>/`.

## Security and privacy

create-course itself does not require API keys.

Generated courses can contain source material, learner profile data, progress
notes, weak areas, NotebookLM exports, and brownfield copies. Do not publish
those files unless you intentionally want them public. Prefer publishing only
`index.html` and approved public assets.

NotebookLM support is optional and uses an unofficial third-party tool when
enabled. Do not upload confidential material unless you understand that tool's
privacy and account behavior.

## Attribution

create-course is hosted at [eugeniawang/create-course](https://github.com/eugeniawang/create-course).
It is a fork of John Brophy's
[johnbr0phy/course-builder](https://github.com/johnbr0phy/course-builder), with
additional inspiration from
[Abiodedeyi's CCA-F reference repo](https://github.com/abiodedeyi/cca-f-exam-prep/tree/master/reference),
[Carl Vellotti's Claude Code PM course](https://github.com/carlvellotti/claude-code-pm-course),
[Matt Pocock's grill-me skill](https://github.com/mattpocock/skills), structured
planning practices, and optional NotebookLM workflows.

## Repo layout

```
create-course/                    (create-course repo)
├── README.md
├── docs/index.html             # the landing page (served by GitHub Pages)
└── course-builder/             # installable create-course skill folder
    ├── SKILL.md
    ├── reference/              # course-anatomy, homepage, deploy-pages recipes
    ├── assets/                 # config-driven homepage shell + Pages workflow
    └── templates/              # scaffolds for every generated course file
```
