---
title: "{{COURSE_TITLE}} Course Plan"
status: "planning"
delivery_environment: "{{claude-code | claude-cowork | codex | multiple}}"
source_policy: "{{source-traceability policy}}"
created: "{{DATE}}"
updated: "{{DATE}}"
---

# {{COURSE_TITLE}} Course Plan

## Identity

- **Build mode:** {{greenfield | brownfield}}
- **Original course path:** {{required for brownfield, else none; read-only}}
- **Reconstruction copy path:** {{path inside this self-contained course folder, required for brownfield}}
- **Preserve from existing course:** {{lessons/assets/runtime conventions to keep}}
- **Original course mutation policy:** Never edit the original course in place.
- **Audience:** {{Who this is for}}
- **Promise:** {{What the learner can do by the end}}
- **Practice environment:** {{Where exercises happen}}
- **Running example:** {{What every learner brings}}
- **Source / inspiration credit:** {{Primary sources and inspiration}}

## Goal Lens

- **Intake template:** {{source-led | goal-led | topic-led}}
- **Goal type:** {{specific outcome | general learning | both}}
- **Specific target:** {{Exam/certification/project/task/onboarding goal, if any}}
- **Course goals:** {{2-4 concrete learner goals}}
- **Success criteria:** {{What proves the course worked}}
- **Out of scope:** {{Topics intentionally excluded to prevent sprawl}}
- **Deadline / external standard:** {{Date, exam blueprint, job requirement, none}}
- **Focusing interview notes:** {{Key answers from the light grill-me goal interview}}

## Runtime Architecture

- **Delivery environment:** {{Claude Code | Claude Cowork | Codex | multiple}}
- **Adapters generated:** {{CLAUDE.md / COWORK.md / AGENTS.md}}
- **Runtime-specific notes:** {{Structured input tools, file conventions, commands}}

## Learning Architecture

- **Beginner-first mode:** {{yes/no/default}}
- **Glossary-first teaching:** {{yes/no}}
- **Competency/domain map:** {{yes/no}}
- **Review gates:** {{lesson/session/week/cumulative}}
- **Capstone:** {{What ships at the end}}
- **NotebookLM / visuals:** {{disabled | optional | required for source-led visuals}}
- **NotebookLM materials exist:** {{yes/no/unknown}}
- **NotebookLM setup choice:** {{connect now | optional later | disabled}}
- **Visual commands active:** {{yes/no}}

## Capabilities

| Capability | Input | Output | Success criteria |
| --- | --- | --- | --- |
| start / continue | learner state | next action | learner knows exactly what to do |
| define | term | plain-language definition | learner can explain/use the term |
| quiz-me | weak/current concepts | scenario checks | misconception logged or cleared |
| practice-exam | competency map | cumulative scenarios | readiness signal + remediation |
| progress | progress files | next action | weak areas and review queue visible |
| show diagram / infographic / mind map | concept + source anchor | visual artifact or fallback explanation | learner sees relationship/process clearly |

## Configuration

| Field | Prompt | Default | Used by |
| --- | --- | --- | --- |
| build_mode | Starting from scratch or reconfiguring an existing course? | greenfield | intake routing |
| original_course_path | Where is the existing course? | none | brownfield read-only source |
| reconstruction_copy_path | Where was the source copied inside this folder? | none | brownfield working source |
| intake_template | Are we starting from source, a goal, or a general topic? | infer, then confirm | intake routing |
| goal_type | Is this for a specific goal, general learning, or both? | general learning | scope and checks |
| course_goals | What 2-4 things should the learner be able to do? | ask during intake | outline and validation |
| delivery_environment | Where will the learner use the course? | ask/recommend | adapter generation |
| beginner_first | Assume zero prior knowledge? | true | lesson pacing/remediation |
| notebooklm_setup | Do you have NotebookLM materials, and connect now or later? | optional later | visuals/materials |

## Dependencies and Guardrails

- {{Practice environment setup}}
- {{Credit/cost/safety limits}}
- {{Source freshness or validation limits}}

## Validation Checklist

- [ ] Delivery environment honored.
- [ ] Goal lens prevents sprawl.
- [ ] Session count matches all generated files.
- [ ] Lesson structure complete for every session.
- [ ] Scenario checks map to competencies/source anchors when relevant.
- [ ] Weak concepts have remediation and review paths.
- [ ] Source credit preserved.
- [ ] JSON validates.

## Build Roadmap

1. {{Plan/source artifacts}}
2. {{Runtime adapters}}
3. {{Lessons and tracking templates}}
4. {{Homepage/deploy, if wanted}}
