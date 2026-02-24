# Schema: confirmed-primitives

Output of `tml-interview`. Input to `tml-infer` and `tml-map`.

Every primitive in this document has been validated by a human participant. Nothing here is a guess.

## Provenance metadata

Every confirmed primitive carries this metadata:

```
name: [Name]
status: confirmed | inferred | gap
source: [What the human said, or "extracted from [document]"]
confirmed_by: [Human name or "pending"]
timestamp: [When confirmed]
```

- **confirmed** — the human explicitly validated this.
- **inferred** — the system concluded this; the human did not reject it but did not explicitly confirm it either. These get flagged in `tml-infer`.
- **gap** — the system identified that something should exist here but could not determine what. Gaps are preserved, not filled.

## Structure

Same primitive types as candidate-primitives, but with provenance metadata on every entry:

```markdown
## Scope

### [Name]
- **Description:** [What this scope covers]
- **Status:** confirmed
- **Source:** "[Direct quote from participant]"
- **Confirmed by:** [Name]
- **Timestamp:** [ISO 8601]

## Domains

### [Name]
- **Description:** [What this domain covers]
- **Ownership:** [Who owns this domain]
- **Status:** confirmed
- **Source:** "[Direct quote]"
- **Confirmed by:** [Name]
- **Timestamp:** [ISO 8601]

## Capabilities

### [Name — verb-noun format]
- **Description:** [What this capability does]
- **Belongs to domain:** [Domain name]
- **Status:** confirmed
- **Source:** "[Direct quote]"
- **Confirmed by:** [Name]
- **Timestamp:** [ISO 8601]

## Archetypes

### [Name]
- **Description:** [Who this archetype is]
- **Can do:** [List]
- **Cannot do:** [List]
- **Status:** confirmed
- **Source:** "[Direct quote]"
- **Confirmed by:** [Name]
- **Timestamp:** [ISO 8601]

## Policies

### [Name]
- **Description:** [What this policy constrains]
- **Constrains:** [What primitive(s)]
- **Status:** confirmed
- **Source:** "[Direct quote]"
- **Confirmed by:** [Name]
- **Timestamp:** [ISO 8601]

## Connectors (read paths)

### [Name]
- **Description:** [What external system]
- **Status:** confirmed
- **Source:** "[Direct quote]"
- **Confirmed by:** [Name]
- **Timestamp:** [ISO 8601]

## Bindings (write paths)

### [Name]
- **Description:** [What external system]
- **Status:** confirmed
- **Source:** "[Direct quote]"
- **Confirmed by:** [Name]
- **Timestamp:** [ISO 8601]

## Views

### [Name]
- **Description:** [What filtered perspective]
- **References capabilities:** [List]
- **Status:** confirmed
- **Source:** "[Direct quote]"
- **Confirmed by:** [Name]
- **Timestamp:** [ISO 8601]

## Provenance Observations

- **Tracked:** [What is currently tracked]
- **Not tracked:** [What is not tracked]
- **Source:** "[Direct quote]"

## Open Questions

- [Question 1 — what could not be resolved during the interview]
- [Question 2]
```

## Rules

- No provenance chain = no primitive in the map.
- Preserve the participant's actual language.
- Do not upgrade "inferred" to "confirmed" without explicit human validation.
- Open questions are carried forward, not silently dropped.
