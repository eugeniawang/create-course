# NotebookLM Integration

Use this optional path when a course is source-led, text-heavy, or would benefit
from diagrams, infographics, mind maps, study guides, quizzes, flashcards, or
downloadable visual artifacts.

This workflow is inspired by `teng-lin/notebooklm-py`, an unofficial NotebookLM
Python/CLI/agent integration. Important caveat: it uses undocumented Google APIs,
is not affiliated with Google, may break, and may hit rate limits. Generated
courses must work without it.

Treat NotebookLM as an external service and `notebooklm-py` as a third-party
dependency. Do not ask learners to upload confidential source material unless they
understand the account, storage, and sharing behavior.

## create-course Behavior

Ask whether the user wants a NotebookLM-backed materials workspace. If yes:

1. Create `NOTEBOOKLM.md`.
2. Add a `materials/` folder plan for source files and generated artifacts.
3. Add runtime commands:
   - `show me a diagram of X`
   - `make an infographic of X`
   - `make a mind map of X`
   - `make study cards for X`
   - `refresh visuals`
4. Keep `SOURCE.md` authoritative. NotebookLM artifacts support learning; they do
   not override the source.

## Setup Notes For Generated Courses

Do not commit credentials. Tell users to authenticate locally if they choose the
NotebookLM path:

```bash
# Optional third-party dependency; review the package before use.
pipx install notebooklm-py
notebooklm login
```

Some users may prefer browser cookies or a specific Google profile. Point them to
the upstream project for current auth options:
`https://github.com/teng-lin/notebooklm-py`

## Materials Layout

```text
materials/
├── sources/           # PDFs, links list, transcripts, docs, notebook exports
├── visuals/           # infographics, diagrams, mind maps
├── study-assets/      # quizzes, flashcards, study guides
└── notebooklm.log.md  # what was generated, from which sources, when
```

## Visual Artifact Rules

- Prefer visuals for relationships, processes, comparison tables, timelines, and
  misconception-heavy topics.
- Do not generate visuals just for decoration.
- Every visual should answer a learner question.
- Store downloaded artifacts under `materials/visuals/` or
  `materials/study-assets/`.
- Record source inputs and validation notes in `notebooklm.log.md`.

## Runtime Command Behavior

When the learner asks for a diagram/infographic/mind map:

1. Identify the concept and source section.
2. Decide whether NotebookLM is configured.
3. If configured, generate or fetch the artifact and link it.
4. If not configured, explain the setup briefly or provide a text-only fallback.
5. Ask one follow-up: "Does this make the relationship clearer, or should I
   simplify it?"
