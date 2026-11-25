# Senior Project 1 — Documentation Requirements

Department template "Senior Project1 Documentation" (original: [senior-project-1-requirements.pdf](senior-project-1-requirements.pdf)). Prof. Kholief asked for it at the 2026-10-20 meeting (`../meeting-notes.md`). **Chapters 1–2 were due ~2026-10-27.** The due dates for chapters 3–4 are not known yet.

**Task lists, one page per step (2026-11-23):** [Chapters 1–2](rased-ch1-2-tasks.pdf) (send Sun 29 Nov), [Chapter 3](rased-ch3-tasks.pdf) (Sun 6 Dec), [Chapter 4 + full document](rased-ch4-tasks.pdf) (Sun 13 Dec). Ch. 3–4 dates are team targets. Sources: `build/tasks/*.html` + `style.css`, exported with headless Edge. Finished parts are attached to their Jira task and the card is moved to Done (D-014); at the end of each sprint the lead collects them for GitHub. Progress is tracked in Jira (space RASED, key RAS; see D-012).

Status as of 2026-10-29. "Source" is where the material already exists, so it can be adapted instead of written from scratch.

## 1. Introduction

| # | Item | Status | Source |
|---|---|---|---|
| 1.a | Motivation | Draft exists | Proposal §1.1, §1.3 |
| 1.b | Problem Statement | Draft exists | Proposal §1.2 |
| 1.c | Goals of the Project | Draft exists | Proposal §1.5 |
| 1.d | Currently Available Solutions | Draft exists | Proposal §3.3–3.6 |
| 1.e | Features Matrix (them vs. us) | Draft exists | Proposal §3.7. Add the unique/multimedia features from the interviews |
| 1.f | Development methodology + why | **Done 2026-11-25** (approved, attached to RAS-4) | `build/chapters/build_1f.js` → `RASED-1f-methodology.docx/.pdf` (local). Scrum, 1-week sprints, Jira (D-012) |

## 2. Requirements Analysis

| # | Item | Status | Source |
|---|---|---|---|
| 2.a | Surveys / questionnaires / interviews | **Interviews only** (no survey, D-013) | `../field-research/` captain interviews |
| 2.b | Results as charts | **Dropped** (no survey, D-013) | — |
| 2.c | Discussion of results | **Missing**: discuss the interview findings | `../field-research/` |
| 2.d | Functional requirements | Draft exists | Proposal §4.3 |
| 2.e | Non-functional requirements | Draft exists | Proposal §4.5 |
| 2.f | Use case diagrams | **Missing** | Actors in proposal §4.1 |

## 3. Design

| # | Item | Status | Source |
|---|---|---|---|
| 3.a | Sequence diagrams | **Missing** | `../architecture.md` §1, §5 (forecast request, ingestion, retrain) |
| 3.b | ERD | **Missing** | `../architecture.md` §3 (medallion layers) |
| 3.c | DFD level 0 and level 1 | **Missing** | `../architecture.md` §1 |
| 3.d | State diagram (optional) | **Missing** | Model lifecycle: challenger → champion → rolled back |
| 3.e | Algorithms / pseudocode | Material exists | `../data-and-mlops.md` (labels, features, retrain/rollback) |
| 3.f | UML class diagrams | **Missing** | — |

## 4. Implementation Aspects

| # | Item | Status | Source |
|---|---|---|---|
| 4.a | Overall system architecture (block diagram) | Text only, **no diagram** | `../architecture.md`, proposal §2.3 |
| 4.b | Tools and technologies shown on the 4.a diagram | Table only | `CLAUDE.md` tech stack |
| 4.c | Prototype (screens) | **Missing** | Proposal §4.2 S1–S9 are placeholders. Must be a full website (2026-10-20 meeting) |
