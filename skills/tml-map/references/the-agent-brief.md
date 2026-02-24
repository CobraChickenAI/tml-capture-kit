# Schema: the-agent-brief

Third artifact from `tml-map`. Operational instructions for AI agents.

This document must be actionable standalone. An agent receiving only this brief — without the map, changelog, or any other context — should know exactly what it can do, what it cannot do, and what rules bind it.

## Structure

```markdown
# Agent Brief: [Scope Name]

## Your Operating Context

[One sentence — what this scope is and what you are operating within]

## The Functional Areas

- **[Domain name]:** [One sentence — what this domain covers]

[Repeat for each domain]

## Capabilities You May Interact With

| Capability | Domain | Constraints |
|-----------|--------|-------------|
| [Name] | [Domain] | [Any limits — rate, approval required, etc.] |

## Your Role

**Archetype:** [Name]
**Description:** [One sentence]

**Can:**
- [Permission 1]
- [Permission 2]

**Cannot:**
- [Boundary 1]
- [Boundary 2]

## Rules That Bind You

| Policy | What It Means For You |
|--------|----------------------|
| [Policy name] | [Specific behavioral constraint in plain language] |

## Systems You May Access

### Read (connectors)
- **[Name]:** [What you can read and when]

### Write (bindings)
- **[Name]:** [What you can write and when]

### Off-Limits
- [System or action that is explicitly not permitted]

## Provenance Requirements

What you must log:
- [Requirement 1 — e.g., every decision with reasoning]
- [Requirement 2 — e.g., every external system interaction]

What you do not need to log:
- [Exception — e.g., read-only lookups]
```

## Rules

- No TML jargon. The agent does not need to know what TML is.
- Every "cannot" must be explicit. Silence is not a boundary.
- Policies must translate to specific behavioral constraints, not abstract rules.
- This document is the agent's operating contract. Ambiguity here becomes unpredictable behavior.
