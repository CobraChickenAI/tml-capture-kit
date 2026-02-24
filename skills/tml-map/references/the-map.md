# Schema: the-map

First artifact from `tml-map`. The primary deliverable.

A plain-language architecture document readable by anyone. No TML jargon. No technical framework language. A non-technical stakeholder should be able to read this and understand their own organization.

## Structure

```markdown
# [Scope Name]

[Opening paragraph — what this is, where its boundaries are, what it serves]

## What We Do

### [Domain Name]
**Owned by:** [Who]

- [Capability — verb-noun, one sentence description]
- [Capability]
- [Capability]

[Repeat for each domain]

## Who's Involved

### [Archetype Name]
[One-sentence description]

**Can:** [List of permissions/capabilities]
**Cannot:** [List of explicit boundaries]

[Repeat for each archetype]

## The Rules

### [Policy Name]
[What it constrains, in plain language]
**Applies to:** [What primitive(s)]

[Repeat for each policy]

## What We Connect To

### Inputs (what we read)
- **[Connector Name]** — [What external system, what data flows in]

### Outputs (what we write)
- **[Binding Name]** — [What external system, what data flows out]

## What Each Role Sees

### [View Name]
**For:** [Which archetype(s)]
[What information is filtered/presented and why]

[Repeat for each view]

## How We Track What Happened

**Currently tracked:** [List]
**Not tracked:** [List with note on risk]

---

*Last updated: [Date]*
*Based on conversation with: [Participant name]*
```

## Rules

- 7th-8th grade reading level.
- Active voice. Short sentences. No hedging.
- Use the participant's actual language, not framework terms.
- Do not invent primitives. If it wasn't confirmed, it doesn't appear.
- Flag gaps honestly — "Not tracked" is more useful than pretending.
