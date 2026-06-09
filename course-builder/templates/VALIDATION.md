# VALIDATION.md — {{COURSE_TITLE}}

> Required maintainer validation record. Structural validation proves the files
> are coherent. Harness proof shows the selected runtime can start/resume the
> course. Adversarial review catches contradictions before a learner hits them.

## Verification Summary

- **Selected delivery harness:** {{Claude Code / Claude Cowork / Codex / combination}}
- **Runtime adapter(s):** {{CLAUDE.md / COWORK.md / AGENTS.md}}
- **Verification date:** {{date}}
- **Verifier:** {{agent/person}}
- **Verdict:** {{pass / pass with caveats / blocked}}
- **Runtime proof level:** {{structural dry-run / live harness smoke test}}

## Structural Validation

- [ ] Required files exist for the selected runtime(s).
- [ ] Session count matches `README.md`, `LESSONS.md`, `PROGRESS.md`, and
      `progress.json`.
- [ ] JSON files parse.
- [ ] Lessons include goal, skill built, guided practice, explain-back, scenario
      check, apply/transfer, and review hooks.
- [ ] Source credit and source anchors are present where needed.
- [ ] Glossary terms are defined before first dependency.
- [ ] Competencies/scenarios map to sessions when `COMPETENCY_MAP.md` exists.
- [ ] NotebookLM artifacts are optional, source-backed, and logged when
      `NOTEBOOKLM.md` exists.
- [ ] No unfilled placeholders remain.
- [ ] `README.md` quick start matches the selected runtime adapter(s).

## Self-contained Folder Audit

- [ ] Learner can start from only this generated folder.
- [ ] No required step depends on the builder repo.
- [ ] No required step depends on the original brownfield folder or private local
      paths.
- [ ] Setup copies every required template into root working files.
- [ ] Practice data, setup notes, and optional external integrations are included
      or clearly marked optional with fallback behavior.

## Harness-specific Smoke Proof

| Harness | Adapter | Commands checked | Tool assumptions | Fallback documented | Proof level | Result |
| --- | --- | --- | --- | --- | --- | --- |
| {{Claude Code / Cowork / Codex}} | {{file}} | start, continue, help, define, progress, review, session navigation | {{structured input / shell / none}} | {{yes/no}} | {{structural/live}} | {{pass/fail}} |

- [ ] `start` creates learner files and begins the right session.
- [ ] `continue` resumes from `progress.json`.
- [ ] `define X` uses or updates `GLOSSARY.md`.
- [ ] Scenario checks record weak concepts, misconceptions, retry status, and
      next action.
- [ ] Review queue resurfaces weak concepts later.
- [ ] Visual commands either return a saved artifact or a clear text-only fallback.

## Adversarial Coherence Review

Review the actual generated folder as a skeptical learner and maintainer.

| Finding | Severity | Disposition | Notes |
| --- | --- | --- | --- |
| {{contradiction / missing prerequisite / broken command / external dependency}} | {{high/medium/low}} | {{fixed / accepted / blocked}} | {{rationale or next action}} |

Checklist:
- [ ] No contradictions between README, LESSONS, SOURCE, progress files, and
      runtime adapter(s).
- [ ] No undefined jargon before first use, or glossary fallback exists.
- [ ] Exercises are possible in the selected practice environment.
- [ ] Scenario checks test judgment/competency, not just recall.
- [ ] Remediation path exists for weak concepts and failed checks.
- [ ] Multi-adapter behavior matches where more than one harness is selected.

## Coverage Gaps

| Gap | Impact | Fix |
| --- | --- | --- |
| {{gap}} | {{impact}} | {{fix}} |

## Maintainer Notes

{{Validation notes, source freshness limits, runtime caveats, or unresolved proof limits.}}
