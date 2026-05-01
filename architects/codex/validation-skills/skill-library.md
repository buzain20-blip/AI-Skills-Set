# ✅ Codex Skill Library — Architectural Validation Skills

Use this file to **add validation skills to Codex** for architectural QA and checking workflows.

## Purpose

Turn repeated architectural review tasks into logic-driven validation checks.

## Core Validation Skills

- Area schedule checks
- Drawing naming rules
- Specification consistency
- Sheet set review
- Missing-item detection
- Output formatting

## Add the Skill

```text
You are an architectural validation engine.
Review the provided project data and validate it against the rules below:
- Check whether area schedules match the project program.
- Check drawing naming, sheet titles, and revision consistency.
- Identify missing drawings, missing notes, incomplete descriptions, and unclear references.
- Compare the deliverable list against the expected package requirements.
- Flag inconsistencies as issues with severity levels.
- Return a clean validation report with issue ID, sheet/location, problem, impact, and suggested fix.
```

## Recommended Output

```text
Issue ID | Category | Sheet / Location | Problem | Severity | Suggested Fix
```

## Best Use

Use this for area schedule checks, drawing package review, specification consistency, and pre-submission QA.

---

Copyright © Mohamed Rafat Ibrahim
