# Schema: candidate-primitives

Output of `tml-extract`. Input to `tml-interview` (warm-start mode).

Candidates are unconfirmed. Every candidate requires human validation before it enters the confirmed set.

## Structure

A markdown document with one section per primitive type. Each section contains zero or more candidates in this format:

```markdown
## Scope Candidates

### [Candidate Name]
- **Description:** [What this scope covers]
- **Confidence:** high | medium | low
- **Source:** [Direct quote or reference to source document]

## Domain Candidates

### [Candidate Name]
- **Description:** [What this domain covers]
- **Confidence:** high | medium | low
- **Source:** [Direct quote or reference to source document]

## Capability Candidates

### [Candidate Name]
- **Description:** [What this capability does — verb-noun format]
- **Belongs to domain:** [Domain name]
- **Confidence:** high | medium | low
- **Source:** [Direct quote or reference to source document]

## Archetype Candidates

### [Candidate Name]
- **Description:** [Who this archetype is]
- **Can do:** [List]
- **Cannot do:** [List]
- **Confidence:** high | medium | low
- **Source:** [Direct quote or reference to source document]

## Policy Candidates

### [Candidate Name]
- **Description:** [What this policy constrains]
- **Constrains:** [What primitive(s) it applies to]
- **Confidence:** high | medium | low
- **Source:** [Direct quote or reference to source document]

## Connector Candidates (read paths)

### [Candidate Name]
- **Description:** [What external system this reads from]
- **Confidence:** high | medium | low
- **Source:** [Direct quote or reference to source document]

## Binding Candidates (write paths)

### [Candidate Name]
- **Description:** [What external system this writes to]
- **Confidence:** high | medium | low
- **Source:** [Direct quote or reference to source document]

## View Candidates

### [Candidate Name]
- **Description:** [What filtered perspective this provides]
- **References capabilities:** [List]
- **Confidence:** high | medium | low
- **Source:** [Direct quote or reference to source document]

## Provenance Observations

- **Existing tracking:** [What is already tracked]
- **Gaps:** [What is not tracked]
- **Source:** [Direct quote or reference to source document]

## Extraction Gaps

- [Gap 1 — what could not be determined from the source material]
- [Gap 2]
```

## Rules

- Every candidate must cite its source.
- Use the source's language, not TML jargon.
- Confidence ratings reflect extraction certainty, not importance.
- Gaps are explicit — do not fill them with assumptions.
