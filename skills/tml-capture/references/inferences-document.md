# Schema: inferences-document

Output of `tml-infer`. Reviewed by human before `tml-map` generates artifacts.

This document separates what the human said from what the system concluded. It is the debugging surface for the entire pipeline.

## Structure

```markdown
# Inferences

## [Primitive Type — e.g., Scope, Domains, Capabilities]

| Element | Status | Source / Reasoning | Confidence | Most Likely Wrong If |
|---------|--------|-------------------|------------|---------------------|
| [Name] | Stated | "[Direct quote]" | High | [Condition] |
| [Name] | Inferred | [System reasoning] | Medium | [Condition] |
| [Name] | Gap | [What's missing] | -- | [Condition] |

[Repeat for each primitive type]

## Inferences to Review

Only Medium and Low confidence items. Presented as a focused list for the human to confirm or correct.

- **[Element name]** — [What was inferred and why]. Confidence: [level]. Correct? [Y/N/Modify]

## Gaps Not Filled

Items the system identified as likely existing but could not determine from available input.

- **[Gap description]** — [What would resolve it]
```

## Three types of inference

1. **Boundary inferences** — the system decided where a scope, domain, or capability ends.
2. **Classification inferences** — the system decided which primitive type something belongs to.
3. **Gap-filling inferences** — the system filled a gap rather than leaving it empty.

## Rules

- Every inference must have a "Most Likely Wrong If" entry.
- Use direct quotes where available.
- Stated vs. Inferred vs. Gap must be clearly distinguished.
- Gaps are flagged, not filled. If the system filled a gap, it is an inference, not a gap.
