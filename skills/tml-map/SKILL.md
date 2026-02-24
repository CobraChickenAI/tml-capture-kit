---
name: tml-map
description: "Generate architecture artifacts from confirmed TML primitives. Produces three outputs: The Map (plain-language architecture document), The Changelog (provenance trail), and The Agent Brief (operational instructions for AI). No jargon. No TML terminology in output. Part of the TML capture pipeline — runs after tml-infer, feeds into tml-so-what."
license: MIT
metadata:
  author: Michael Schartung
  repository: CobraChickenAI/tml-capture-kit
  version: "1.0.0"
  pipeline: tml-capture-pipeline
  pipeline-stage: "4"
  inputs: "confirmed-primitives, inferences-document"
  outputs: "the-map, the-changelog, the-agent-brief"
  schema-ref: "references/the-map.md, references/the-changelog.md, references/the-agent-brief.md"
---

# TML Map

Takes confirmed primitives (from `tml-interview`) and produces three artifacts that make architecture visible, traceable, and operational.

## When to Use

- After `tml-interview` has produced confirmed primitives
- As part of a `tml-capture` orchestration
- When updating an existing map with new confirmed primitives

## The Three Artifacts

### 1. The Map

A plain-language architecture document that anyone can read. This is the primary deliverable — the thing someone prints, shares in Slack, or opens in a meeting.

**Critical rule: No TML jargon.** The map uses everyday language. The reader should never need to know what TML is to understand their own architecture.

**Mapping from primitives to plain language:**

| Primitive | Map Heading | What It Says |
|-----------|-------------|-------------|
| Scope | *[Name of the thing]* | Opening paragraph — what this is and where its boundaries are |
| Domains | **What We Do** | Functional areas with clear ownership |
| Capabilities | *(nested under domains)* | What each area actually does — the verbs |
| Archetypes | **Who's Involved** | Roles and what each can do |
| Policies | **The Rules** | What must hold, what's off-limits |
| Connectors | **What We Read From** | External inputs and data sources |
| Bindings | **What We Write To** | External outputs and effects |
| Views | **What Each Role Sees** | How information is filtered per audience |
| Provenance | **How We Track What Happened** | What's tracked, what's not |

**Format:**

```markdown
# [Scope Name]

[One paragraph: what this is, who it serves, what boundary it defines.]

---

## What We Do

### [Domain 1 Name]
*Owned by: [owner]*

- **[Capability 1]** — [what it does, what outcome it delivers]
- **[Capability 2]** — [what it does, what outcome it delivers]

### [Domain 2 Name]
*Owned by: [owner]*

- **[Capability 3]** — [what it does, what outcome it delivers]

---

## Who's Involved

### [Archetype 1]
[Description of this role]

**Can:** [list of what they can do]
**Cannot:** [list of restrictions]

### [Archetype 2]
[Description]

**Can:** [list]
**Cannot:** [list]

---

## The Rules

1. **[Policy name]** — [plain language description of the rule]
   *Applies to: [what this constrains]*

2. **[Policy name]** — [plain language description]
   *Applies to: [what this constrains]*

---

## What We Connect To

### Inputs (what we read from)
- **[Connector 1]** — [what data, from where]
- **[Connector 2]** — [what data, from where]

### Outputs (what we write to)
- **[Binding 1]** — [what effect, to where]
- **[Binding 2]** — [what effect, to where]

---

## What Each Role Sees

- **[Archetype 1]** sees: [what's visible to them]
- **[Archetype 2]** sees: [what's visible to them]

---

## How We Track What Happened

**Currently tracked:**
- [what provenance exists]

**Not yet tracked:**
- [gaps identified during interview]

---

*Last updated: [date]*
*Based on conversation with: [participant]*
```

**Style rules:**
- Write at 7th-8th grade reading level
- Use their words, not yours. If they said "the sales team," write "the sales team."
- Short sentences. Active voice. No hedging.
- If something is unclear, say so directly: "This needs clarification."
- No bullet point should exceed two lines

### 2. The Changelog

A provenance trail that traces every element of the map back to where it came from. This is the artifact that makes the map *trustworthy* — not just a document, but a document you can audit.

**Format:**

```markdown
# Changelog: [Scope Name]

**Created:** [date]
**Source material:** [list of documents provided, if any]
**Interviewed:** [participant name]
**Method:** [warm-start from extraction | cold-start interview]

---

## How This Map Was Built

[One paragraph explaining the process: what was provided, what was extracted, what was confirmed in conversation.]

---

## Provenance by Section

### Scope: [name]
- **Source:** [extracted from document X | stated by participant at time T]
- **Status:** Confirmed

### Domain: [name]
- **Source:** [where this came from]
- **Status:** [Confirmed | Inferred — awaiting confirmation | Needs clarification]

### Capability: [name]
- **Source:** [exact quote or document reference]
- **Status:** [status]
- **Note:** [any context about how this was identified]

[...continue for every primitive in the map...]

---

## Open Items

| Item | Type | Status | Notes |
|------|------|--------|-------|
| [description] | [gap/question/conflict] | Open | [context] |

---

## Revision History

| Date | Change | Source |
|------|--------|--------|
| [date] | Initial map created | Interview with [participant] |

---

*This changelog is the audit trail for the architecture map. Every element traces back to a source.*
```

### 3. The Agent Brief

Operational instructions that tell an AI agent how to work within this architecture. This is the artifact that makes the map *actionable* — not just documentation, but a set of bounds an agent can operate from.

**Format:**

```markdown
# Agent Brief: [Scope Name]

**Effective:** [date]
**Based on:** Architecture map v[version], confirmed by [participant]

---

## Your Operating Context

You are operating within [scope name]. [One sentence about what this is.]

---

## What You Need to Know

### The Functional Areas

[For each domain, one sentence about what it does and who owns it.]

### The Capabilities You May Interact With

[For each capability relevant to agent work:]
- **[Capability]**: [what it does]. [Any constraints on how to interact with it.]

---

## Your Role

You are operating as: **[archetype name, or "to be assigned"]**

**You can:**
- [explicit list of allowed actions]

**You cannot:**
- [explicit list of prohibited actions]

**When uncertain:** [what to do — ask, escalate, stop]

---

## Rules That Bind You

[For each policy relevant to agent behavior:]

1. **[Policy]**: [plain language]. This means: [what the agent must do or not do, specifically].

---

## Systems You May Access

**Read from (Connectors):**
- [system]: [what data, how to access]

**Write to (Bindings):**
- [system]: [what effects, how to commit]

**Off-limits:**
- [anything explicitly not accessible]

---

## Provenance Requirements

When you take actions within this scope:
- [what must be logged]
- [what attribution is required]
- [what audit trail expectations exist]

---

*This brief was generated from an architecture interview. It reflects the declared structure as of [date]. If reality has changed, this brief needs updating.*
```

---

## Generation Rules

1. **Use their language.** The map should sound like the person who described it, not like an architecture textbook.
2. **Don't invent.** If a primitive was flagged as a gap in the interview, note it as a gap in the map. Don't fill it with assumptions.
3. **Be honest about confidence.** If something was inferred but not confirmed, say so in the changelog. The map itself should read confidently, but the changelog must be honest.
4. **Keep the map scannable.** Someone should be able to read the map in 5 minutes and understand the shape of the thing. Push detail to the changelog and agent brief.
5. **The agent brief must be actionable.** An AI agent reading just the agent brief (without the map or changelog) should be able to operate safely within bounds. If it can't, the brief isn't done.

## Handling Updates

When generating an updated map (new information added to an existing one):

1. Preserve the existing changelog history
2. Add new entries with the current date and source
3. Mark changed sections in the revision history
4. Highlight what changed since the last version in a "What's New" section at the top of the map

---

## Handoff

The Map, Changelog, and Agent Brief feed into `tml-so-what` for opportunity analysis. They can also stand alone as deliverables.

---

*v1.0 — Built for workplace deployment. Plain language, no jargon, three artifacts.*
