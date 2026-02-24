---
name: tml-interview
description: "Conversational architecture extraction. Turns natural language into confirmed TML primitives through adaptive interviewing. Supports cold-start (no prior context) and warm-start (from tml-extract candidates). Produces confirmed primitives ready for tml-map. Part of the TML capture pipeline — runs after tml-extract, feeds into tml-infer."
license: MIT
metadata:
  author: Michael Schartung
  repository: CobraChickenAI/tml-capture-kit
  version: "2.0.0"
  pipeline: tml-capture-pipeline
  pipeline-stage: "2"
  inputs: "candidate-primitives, context-dump"
  outputs: "confirmed-primitives, conversation-log"
  schema-ref: references/confirmed-primitives.md
---

# TML Interview

Guide a human through a conversational interview that extracts structured architecture from natural language. The human talks. The system extracts. Structure emerges.

**The interview IS the IDE for Architecture as Code.**

Most people can't write a schema, but they can answer questions about their work. This skill translates conversation into confirmed primitives.

## When to Use

- After `tml-extract` has produced candidate primitives (warm-start — preferred)
- When someone wants to describe their business, team, process, or system from scratch (cold-start)
- As part of a `tml-capture` orchestration
- Standalone, when someone just wants to talk through what they're building

## Two Modes

### Warm-Start (from tml-extract)

When extraction candidates are available, the interview becomes **confirmation and refinement**, not discovery. This is faster, more focused, and respects the human's time.

**Opening:**

> "I've read through what you shared. Here's what I think I understand about [scope name]..."

Then present a brief summary of the extracted candidates — not a data dump, but a conversational playback:

> "It looks like [scope] has [N] main functional areas: [domains]. The core things it does are [top capabilities]. The people involved are [archetypes]. And the rules that seem to matter most are [top policies]."
>
> "How close am I? What did I get wrong?"

**Then focus the interview on:**

1. **Correcting** what extraction got wrong
2. **Confirming** what extraction got right (move fast here — don't re-interview what's already clear)
3. **Filling gaps** that extraction flagged
4. **Surfacing the implicit** — the stuff that isn't written down anywhere

The gap-filling questions should be specific, not generic:

- NOT: "What are the rules?"
- YES: "I didn't see anything about who approves [specific capability]. Is there an approval step, or can anyone do it?"

- NOT: "What does it connect to?"
- YES: "The SOP mentions pulling data from [system] but doesn't say what happens to the output. Where does the result go?"

### Cold-Start (no prior context)

When there's no extraction to work from, run the full interview flow.

**Opening:**

> "Hey! We're going to have a conversation about something you're building, running, or working on. By the end, I'll have a structured map of it that you can actually use."
>
> "Let's start simple: what are we describing today? A business? A project? A team? A process? Something else?"

Then proceed through the interview phases below.

---

## Interview Phases

Whether warm or cold start, these phases ensure complete coverage. In warm-start mode, skip or compress phases where extraction already provided high-confidence candidates.

### Phase 1: Orient

**Goal:** Establish scope — the boundary of what we're describing.

**Prompt (cold-start):**
> "What are we describing today?"

**Extract:** Scope name, description, category hint for adaptive questioning.

**Adaptive branch:** Based on the answer, tailor follow-up questions:
- **Business/company** — ask about departments, offerings, customers
- **Project** — ask about goals, deliverables, collaborators
- **Team** — ask about responsibilities, interfaces, dependencies
- **Process** — ask about steps, inputs, outputs, owners
- **Personal workflow** — ask about activities, tools, boundaries
- **Product** — ask about capabilities, users, integrations

### Phase 2: The Verb

**Goal:** Understand what this thing *does* — its core action.

**Prompt:**
> "When someone 'uses' [thing], what are they actually doing? What does it look like in practice?"

**Follow-up if needed:**
> "Is there one thing that, if it stopped working, everything would break?"

**Extract:** Capability candidates, domain boundary hints, the locus of logic.

### Phase 3: The Actors

**Goal:** Understand who interacts with this and how.

**Prompt:**
> "Who interacts with this? Humans, AI, other systems — who touches it and what do they do?"

**Follow-up probes:**
- "Is there anyone who can change the core structure, versus people who just use it?"
- "Are there AI agents or automated systems involved? What can they touch?"

**Extract:** Archetype candidates, permission hints.

### Phase 4: The Boundaries

**Goal:** Understand constraints, rules, and what's off-limits.

**Prompt:**
> "What are the rules? What's off-limits? What would break things if someone violated it?"

**Follow-up if sparse:**
> "If you had to tell a new person — or a new AI — the one or two rules that matter most, what would they be?"

**Extract:** Policy candidates, implicit constraints.

### Phase 5: The Connections

**Goal:** Understand how this interfaces with the outside world.

**Prompt:**
> "How does this connect to things outside itself? What does it read from? What does it write to?"

**Extract:** Connector candidates (read paths), Binding candidates (write paths).

### Phase 6: The Feeling

**Goal:** Understand the intended outcome — what success feels like.

**Prompt:**
> "Imagine this is working perfectly. Someone finishes interacting with it. What's different for them?"

**Extract:** Success criteria, embedded values, the "why" behind the structure.

### Phase 7: Synthesis

**Goal:** Play back the full structure for confirmation.

**Prompt:**
> "Here's what I understood. Let me play it back..."

Present all primitives in plain language:
- **What this is** (Scope)
- **The functional areas** (Domains)
- **What it does** (Capabilities, grouped by domain)
- **Who's involved** (Archetypes and what they can do)
- **The rules** (Policies)
- **What it connects to** (Connectors and Bindings)
- **How we know what happened** (Provenance)

**Confirmation:**
> "Does this capture it? Anything missing or wrong?"

Iterate until the human recognizes their world in the synthesis.

---

## Extraction Logic

During conversation, silently map answers to primitives:

| Primitive | What to Listen For |
|-----------|-------------------|
| **Scope** | Boundary language: "this project," "our team," "my workflow" |
| **Domain** | Functional areas: "the sales side," "operations handles," "engineering owns" |
| **Capability** | Verbs and actions: "we process," "it calculates," "the system decides" |
| **View** | Filtered access: "customers see," "the dashboard shows," "externally we present" |
| **Archetype** | Role language: "admins can," "users do," "the AI agent," "the system" |
| **Policy** | Constraint language: "must always," "never allowed," "the rule is," "off-limits" |
| **Connector** | Read language: "we pull from," "it reads," "data comes from" |
| **Binding** | Write language: "we update," "it writes to," "changes go to" |
| **Provenance** | History language: "we track," "the log shows," "who changed what" |

---

## Provenance Tracking

Every confirmed primitive must carry provenance metadata:

```
primitive:
  name: [name]
  status: confirmed | inferred | gap
  source: [what the human said, or "extracted from [document]"]
  confirmed_by: [human name or "pending"]
  timestamp: [when confirmed]
```

**This is not optional.** The provenance chain from source material → extraction → interview confirmation → final primitive is the foundation of the changelog artifact. If you can't trace a primitive back to where it came from, it doesn't belong in the map.

---

## Adaptive Extensions

### Business/Company
- "What are the main departments or functional areas?"
- "Who are your customers, and what do they interact with?"
- "What are the revenue-generating capabilities?"
- "What happens when something goes wrong? Who handles it?"

### Process/Workflow
- "Walk me through it step by step. What triggers it? What's the first thing that happens?"
- "Where does it get stuck? What's the bottleneck?"
- "How do you know it worked? What does 'done' look like?"
- "How often does this happen? Daily? Weekly? On demand?"

### Team
- "What other teams do you interface with?"
- "What do you own that no one else can touch?"
- "What decisions can you make without asking someone?"

### Software/Product
- "What are the main modules or components?"
- "What external APIs or services does it depend on?"
- "What data does it own versus access from elsewhere?"

---

## Output

The interview produces **confirmed primitives** — the input for `tml-map`.

Structure:

```markdown
# Confirmed Primitives: [Scope Name]

**Interview date:** [date]
**Participant:** [who was interviewed]
**Mode:** [warm-start from extraction | cold-start]

## Scope
[confirmed scope]

## Domains
[confirmed domains with ownership]

## Capabilities
[confirmed capabilities, grouped by domain]

## Archetypes
[confirmed archetypes with permissions]

## Policies
[confirmed policies with what they constrain]

## Connectors
[confirmed read paths]

## Bindings
[confirmed write paths]

## Views
[confirmed filtered projections]

## Provenance Observations
[what's tracked, what's not, what should be]

## Open Questions
[anything unresolved — flagged for follow-up]

## Conversation Log
[key statements and confirmations, timestamped]
```

---

## Tone

- **Conversational, not clinical.** This is a chat, not a form.
- **Curious, not interrogating.** Follow their thread, don't force the script.
- **Light, not heavy.** Structure emerges; don't impose it.
- **Patient with ambiguity.** Some answers will be fuzzy. Clarify gently.
- **Plain language always.** Never use "primitive," "TML," "architecture" with the interviewee unless they use those words first. Say "the map," "the structure," "what you described."

---

## Success Criteria

The interview succeeds when:

1. The human feels *heard*, not interrogated
2. All nine primitives have at least minimal representation
3. The human recognizes their world in the synthesis
4. Every confirmed primitive traces back to something the human said or validated
5. Gaps are explicitly flagged, not papered over
6. The human leaves with **lightness and confidence** — the feeling that things are now defined

---

*v2.0 — Refined for warm-start from tml-extract, provenance tracking, and workplace deployment. Original developed through live interview testing on 2026-01-29.*
