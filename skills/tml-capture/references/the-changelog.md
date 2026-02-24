# Schema: the-changelog

Second artifact from `tml-map`. The provenance trail.

Every element in the map traces back to a source through this document. If someone asks "where did this come from?" the changelog answers.

## Structure

```markdown
# Changelog

## How This Map Was Built

[Process narrative — what documents were provided, how the interview was conducted, who participated, what mode (warm/cold start)]

## Source Registry

| Source ID | Type | Description |
|-----------|------|-------------|
| S1 | Document | [Document name/description] |
| S2 | Interview | [Participant name, date] |
| S3 | Document | [Document name/description] |

## Provenance by Section

### [Section name from the map — e.g., "What We Do"]

| Element | Status | Source | Notes |
|---------|--------|--------|-------|
| [Domain name] | Confirmed | S2 | "[Quote]" |
| [Capability name] | Inferred | S1 | Extracted from [doc section] |
| [Capability name] | Needs clarification | -- | Participant unsure |

[Repeat for each section of the map]

## Open Items

| Item | Type | Source | Blocking? |
|------|------|--------|-----------|
| [Description] | Gap | S2 | No |
| [Description] | Conflict | S1, S2 | Yes |

## Revision History

| Date | Change | Source | Author |
|------|--------|--------|--------|
| [Date] | Initial map created | S1, S2 | [System/human] |
```

## Rules

- Every major element in the map must have a corresponding entry here.
- Source IDs link back to the source registry.
- Status is: Confirmed, Inferred, or Needs Clarification.
- Preserve revision history across updates — never overwrite, only append.
