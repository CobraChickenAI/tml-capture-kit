# Schema: conversation-log

Secondary output of `tml-interview`. Preserved for provenance and audit.

## Structure

```markdown
# Conversation Log

**Participant:** [Name]
**Date:** [ISO 8601]
**Mode:** warm-start | cold-start
**Duration:** [Approximate minutes]

## Key Statements

| Timestamp | Phase | Statement | Primitive(s) Affected |
|-----------|-------|-----------|----------------------|
| [Time] | Orient | "[Direct quote]" | Scope: [name] |
| [Time] | The Verb | "[Direct quote]" | Capability: [name] |
| ... | ... | ... | ... |

## Corrections

| Timestamp | What Changed | From | To | Participant Said |
|-----------|-------------|------|-----|-----------------|
| [Time] | Domain: [name] | [old] | [new] | "[Direct quote]" |

## Unresolved

- [Topic that was raised but not resolved]
```

## Rules

- Log captures the human's words, not the system's interpretation.
- Every correction is tracked with before/after.
- Unresolved items carry forward as open questions in confirmed-primitives.
