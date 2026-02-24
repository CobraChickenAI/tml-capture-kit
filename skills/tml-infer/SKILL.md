---
name: tml-infer
description: "Surfaces inferences made during a TML interview — the gap between what was explicitly stated and what the system concluded. Produces a structured Inferences document that separates confirmed facts from model-generated conclusions, flags low-confidence inferences for human review, and makes the reasoning chain visible. Part of the TML capture pipeline — runs after tml-interview, before tml-map."
license: MIT
metadata:
  author: Michael Schartung
  repository: CobraChickenAI/tml-capture-kit
  version: "1.0.0"
  pipeline: tml-capture-pipeline
  pipeline-stage: "3"
  inputs: confirmed-primitives
  outputs: inferences-document
  schema-ref: references/inferences-document.md
---

# TML Infer

Takes the confirmed primitives from a `tml-interview` and makes the model's reasoning visible.

Every interview produces two kinds of output: things the human explicitly said, and things the system concluded from what the human said. The map conflates these by design — it reads as authoritative. This skill separates them. It answers: *what did I decide on your behalf, and how confident am I that I got it right?*

This is the debugging surface. If the inference layer is wrong, the map is wrong. Catching it here is cheaper than catching it six months from now.

## When to Use

- After `tml-interview`, before presenting the map for confirmation
- As part of a `tml-capture` orchestration (runs automatically between interview and map)
- When a human wants to audit what the system concluded vs. what they actually said
- As a standalone review of any existing TML map — "show me what you inferred"

---

## What Gets Inferred

Inferences happen in three ways during a TML interview:

### 1. Boundary inferences
The human described something. The system had to decide where the edge of the scope is. That decision is an inference.

*Example: Human said "my team." System inferred the scope as "the Customer Success team, excluding shared services." That boundary is a conclusion, not a quote.*

### 2. Classification inferences
The human said something. The system had to decide which primitive it belongs to. That classification is an inference.

*Example: Human said "we send weekly status emails." System classified that as a Binding (write path to external stakeholders). It could also have been a Capability. The choice was a conclusion.*

### 3. Gap-filling inferences
Something wasn't addressed in the interview. The system either flagged it as a gap or filled it from context. Both are inferences.

*Example: The human never mentioned who can change the approval threshold. System inferred it defaults to the manager archetype based on general patterns. That's a conclusion — and it might be wrong.*

---

## The Analysis

For each primitive in the confirmed primitive set, examine:

1. **What was explicitly stated?** Direct quotes or clear paraphrases from the interview.
2. **What was concluded?** The system's interpretation, classification, or extension of what was stated.
3. **How confident is that conclusion?** High (obvious from context), Medium (reasonable but could be wrong), Low (speculation or pattern-matching).
4. **What would falsify it?** The single most likely way this inference is wrong.

---

## Output Format

```markdown
# Inferences: [Scope Name]

**Interview date:** [date]
**Participant:** [name]
**Generated:** [date]

---

## How to Read This

This document shows what the system concluded vs. what you actually said.

- **Stated** = you said this, or confirmed it when played back
- **Inferred** = the system concluded this from context
- **Gap** = this wasn't addressed; the system flagged it rather than filling it

Review the inferences. If any are wrong, correct them before the map is finalized. A wrong inference in the map is harder to find later.

---

## Scope

**[Scope name]**

| Element | Status | Source / Reasoning |
|---------|--------|--------------------|
| Name | Stated / Inferred | [quote or reasoning] |
| Boundary | Stated / Inferred | [what was said, what was concluded about the edge] |
| Description | Stated / Inferred | [quote or reasoning] |

**Confidence:** High / Medium / Low
**Most likely wrong if:** [what would falsify this]

---

## Domains

### [Domain 1 name]

| Element | Status | Source / Reasoning |
|---------|--------|--------------------|
| Name | Stated / Inferred | [quote or how the name was derived] |
| Ownership | Stated / Inferred / Gap | [who owns this and how we know] |
| Accountability boundary | Stated / Inferred | [what's in vs. out of this domain] |

**Confidence:** High / Medium / Low
**Most likely wrong if:** [what would falsify this]

[repeat for each domain]

---

## Capabilities

### [Capability 1 name]

| Element | Status | Source / Reasoning |
|---------|--------|--------------------|
| Name (verb-noun) | Stated / Inferred | [quote or how the name was derived from what was said] |
| Outcome | Stated / Inferred | [what was said about what this produces] |
| Domain assignment | Stated / Inferred | [why it was placed in this domain] |

**Confidence:** High / Medium / Low
**Most likely wrong if:** [what would falsify this]

[repeat for each capability]

---

## Archetypes

### [Archetype name]

| Element | Status | Source / Reasoning |
|---------|--------|--------------------|
| Role name | Stated / Inferred | [quote or derivation] |
| Can-do list | Stated / Inferred | [what was explicit vs. concluded from context] |
| Cannot-do list | Stated / Inferred / Gap | [what was explicit vs. defaulted from policies] |

**Confidence:** High / Medium / Low
**Most likely wrong if:** [what would falsify this]

[repeat for each archetype]

---

## Policies

### [Policy name]

| Element | Status | Source / Reasoning |
|---------|--------|--------------------|
| Rule statement | Stated / Inferred | [quote or paraphrase] |
| What it constrains | Stated / Inferred | [explicit vs. concluded from context] |
| Enforcement mechanism | Stated / Inferred / Gap | [how this is actually upheld] |

**Confidence:** High / Medium / Low
**Most likely wrong if:** [what would falsify this]

[repeat for each policy]

---

## Connectors & Bindings

### [Connector/Binding name]

| Element | Status | Source / Reasoning |
|---------|--------|--------------------|
| Direction (read/write) | Stated / Inferred | [how the read/write distinction was determined] |
| What flows | Stated / Inferred | [what data or effect, and how we know] |
| External endpoint | Stated / Inferred / Gap | [named system or vague reference] |

**Confidence:** High / Medium / Low
**Most likely wrong if:** [what would falsify this]

[repeat for each connector and binding]

---

## Views

[If no views were identified, state: "No views were identified in this interview. This may be a gap — consider whether different roles see different filtered versions of the same information."]

---

## Provenance

| Element | Status | Source / Reasoning |
|---------|--------|--------------------|
| Existing tracking | Stated / Inferred / Gap | [what was mentioned vs. assumed] |
| Gaps | Stated / Inferred | [what's not tracked and how we know] |

---

## Summary: Inferences to Review

The following inferences are Medium or Low confidence and should be explicitly confirmed before the map is finalized:

| Primitive | Inference | Confidence | What to ask |
|-----------|-----------|------------|-------------|
| [type] | [what was concluded] | Medium/Low | [exact question to confirm or refute] |

---

## Gaps Not Filled

The following were not addressed in the interview. They are flagged rather than inferred:

| Primitive | What's missing | Suggested question |
|-----------|---------------|-------------------|
| [type] | [what's absent] | [how to get the answer] |

---

*This document is not the map. It is the reasoning behind the map. Correct the inferences here, and the map corrects with them.*
```

---

## Rules

1. **Never present an inference as a fact.** If it was concluded rather than stated, say so.
2. **Low confidence is not a failure.** It is the system being honest. Flag it and move on.
3. **The "most likely wrong if" field is mandatory.** If you can't articulate how an inference could be wrong, it means you haven't thought about it carefully enough.
4. **Gaps are better than guesses.** An honest gap is more valuable than a wrong inference that makes it into the map.
5. **Use direct quotes where available.** The provenance chain from statement → inference → primitive is the point.

---

## Handoff

The Inferences document is reviewed by the human before `tml-map` is run. Corrections to inferences propagate to the map automatically. In a `tml-capture` orchestration, the Inferences document is presented alongside the map as the fourth artifact, not before it — the map is the deliverable, the inferences are the audit surface.

---

*v1.0 — The reasoning layer. Makes model conclusions visible so humans can correct them before they compound.*
