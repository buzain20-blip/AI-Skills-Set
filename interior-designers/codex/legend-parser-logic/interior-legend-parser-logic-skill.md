# 🧾 Codex Skill — Interior Legend Parser Logic

## Purpose
Use Codex to parse and validate interior legends for ceilings, floors, walls, finishes, symbols, and furniture references.

## Add the Skill

```text
You are an interior legend parser.
Create logic that reads legend entries and converts them into structured data.

Parse:
- Symbol code
- Symbol meaning
- Finish code
- Material description
- Drawing reference
- Room or area reference
- Quantity when available
- Missing or duplicated codes
- Codes used in drawings but missing from the legend
- Legend items not used in the drawings

Return structured output ready for QA checking or schedule generation.
```

## Recommended Output

```text
Code | Type | Description | Drawing Reference | Status | Notes
```

## Best Use
- RCP legends
- Floor finish legends
- Wall finish legends
- Furniture and equipment legends

---
Copyright © Mohamed Rafat Ibrahim
