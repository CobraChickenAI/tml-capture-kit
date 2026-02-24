---
name: tml-extract
description: "Extract TML primitive candidates from context dumps — documents, SOPs, wiki pages, meeting notes, or any unstructured content about a business process, team, or system. Produces structured candidate primitives for interview confirmation. Part of the TML capture pipeline — runs first, feeds into tml-interview."
license: MIT
metadata:
  author: Michael Schartung
  repository: CobraChickenAI/tml-capture-kit
  version: "1.0.0"
  pipeline: tml-capture-pipeline
  pipeline-stage: "1"
  inputs: context-dump
  outputs: candidate-primitives
  schema-ref: references/candidate-primitives.md
---

# TML Extract

Takes unstructured content about a business, process, team, or system and extracts candidate TML primitives. The output is a structured set of candidates — not confirmed architecture. Candidates require human confirmation via the `tml-interview` skill.

## When to Use

- Someone provides documents, SOPs, wiki pages, or other content describing how something works
- You need to bootstrap an architecture map from existing documentation
- You want to accelerate a TML interview with a warm start instead of cold discovery
- You're preparing for a `tml-capture` orchestration

## Input

Accept any combination of:
- Pasted text (SOPs, process docs, job descriptions, wiki exports)
- File contents (markdown, plain text, HTML, PDF text)
- Meeting notes or transcripts
- Slack/email thread summaries
- Org charts or team descriptions
- Existing architecture diagrams described in text

**There is no wrong input.** If it has signal about what something is, who does it, how it works, or what the rules are — it's valid input.

## Extraction Process

### Step 1: Read Everything First

Do not extract while reading. Read all provided content completely before beginning extraction. You need the full picture to identify boundaries correctly.

### Step 2: Identify the Scope

What is the boundary of what's being described? This is the outermost container.

Listen for:
- "Our team," "this department," "the process," "my business"
- Organizational names, project names, product names
- Anything that defines the edge of what's being mapped

Extract:
```
SCOPE CANDIDATE:
  name: [name]
  description: [one sentence]
  confidence: [high/medium/low]
  source: [which document or section this came from]
```

### Step 3: Identify Domains

What are the functional areas with distinct accountability? Not org chart boxes — outcome-based areas.

Listen for:
- Department or team names that own outcomes
- Functional areas: "sales handles," "ops manages," "engineering builds"
- Accountability language: "responsible for," "owns," "runs"

Extract for each:
```
DOMAIN CANDIDATE:
  name: [name]
  description: [what outcome this area is accountable for]
  confidence: [high/medium/low]
  source: [which document or section]
```

### Step 4: Identify Capabilities

What does this thing actually DO? What are the atomic units of value?

Listen for:
- Verbs: "processes," "calculates," "approves," "generates," "reviews"
- Process steps that produce outcomes
- Things that would break everything if they stopped working
- Value-producing activities (not just tasks)

Extract for each:
```
CAPABILITY CANDIDATE:
  name: [verb-noun format, e.g., "Process Customer Returns"]
  description: [what outcome this delivers]
  belongs_to_domain: [which domain candidate, if identifiable]
  confidence: [high/medium/low]
  source: [which document or section]
```

**Important:** A Capability is not a task, not a tool, not a job title. It's the unit of value — the thing that produces an outcome. "Send email" is a task. "Notify Customer of Shipment Status" is a Capability.

### Step 5: Identify Archetypes

Who interacts with this? What roles exist?

Listen for:
- Job titles (translate to roles, not individuals)
- "Users," "admins," "managers," "customers," "vendors"
- AI agents or automated systems mentioned
- Permission language: "only managers can," "customers see"

Extract for each:
```
ARCHETYPE CANDIDATE:
  name: [role name]
  description: [what this role does in context]
  can_do: [list of actions/capabilities they interact with]
  cannot_do: [list of restrictions, if mentioned]
  confidence: [high/medium/low]
  source: [which document or section]
```

### Step 6: Identify Policies

What are the rules? What must always hold?

Listen for:
- "Must," "always," "never," "required"
- Approval workflows: "needs sign-off," "manager approval"
- Thresholds: "over $500," "more than 3 days"
- Compliance or regulatory mentions
- Implicit rules (things described as "how we do it" that are actually constraints)

Extract for each:
```
POLICY CANDIDATE:
  name: [short name]
  description: [the rule in plain language]
  constrains: [which capabilities or archetypes this applies to]
  confidence: [high/medium/low]
  source: [which document or section]
```

### Step 7: Identify Connectors and Bindings

What does this read from? What does it write to?

Listen for:
- External systems: "we pull from Salesforce," "data comes from the warehouse"
- Integrations: "syncs with," "imports from," "exports to"
- Handoffs: "sends to," "receives from," "passes to"
- Read vs write distinction: pulling data IN (Connector) vs pushing effects OUT (Binding)

Extract for each:
```
CONNECTOR CANDIDATE (read path):
  name: [name]
  description: [what data flows in and from where]
  confidence: [high/medium/low]
  source: [which document or section]

BINDING CANDIDATE (write path):
  name: [name]
  description: [what effect is committed and to where]
  confidence: [high/medium/low]
  source: [which document or section]
```

### Step 8: Identify Views

How is information filtered or projected for different audiences?

Listen for:
- "The dashboard shows," "customers see," "the report includes"
- Different presentations of the same data for different people
- Filtered access: summary vs detail, internal vs external

Extract for each:
```
VIEW CANDIDATE:
  name: [name]
  description: [what it shows and for whom]
  references_capabilities: [which capabilities it projects]
  confidence: [high/medium/low]
  source: [which document or section]
```

### Step 9: Note Provenance Patterns

How is history tracked? What audit/traceability exists (or is missing)?

Listen for:
- "We track," "the log shows," "we record"
- Change history, audit trails, approval records
- **Absence of tracking** — this is equally important to note

Extract:
```
PROVENANCE OBSERVATIONS:
  existing_tracking: [what's currently tracked, if anything]
  gaps: [what should be tracked but isn't]
  source: [which document or section]
```

### Step 10: Identify Gaps

After extraction, explicitly list what's MISSING. This is critical for the interview phase.

```
EXTRACTION GAPS:
  - [primitive type]: [what's unclear or absent]
  - [primitive type]: [what needs human confirmation]
```

Common gaps:
- Policies that are implicit but never written down
- Ownership that's assumed but not declared
- Connectors/Bindings that exist but aren't documented
- Archetypes that are real but not in the org chart

## Output Format

Present extraction results in this structure:

```markdown
# Extraction Results: [Scope Name]

**Source material:** [list of documents/content analyzed]
**Extraction date:** [date]
**Status:** Candidates only — requires interview confirmation

## Scope
[scope candidate]

## Domains ([count] identified)
[domain candidates]

## Capabilities ([count] identified)
[capability candidates, grouped by domain where possible]

## Archetypes ([count] identified)
[archetype candidates]

## Policies ([count] identified)
[policy candidates]

## Connectors ([count] identified)
[connector candidates]

## Bindings ([count] identified)
[binding candidates]

## Views ([count] identified)
[view candidates]

## Provenance
[provenance observations]

## Gaps & Interview Questions
[list of gaps with specific questions to ask in the interview]
```

## Confidence Levels

- **High**: Explicitly stated in source material, unambiguous
- **Medium**: Implied or partially stated, reasonable inference
- **Low**: Speculative, based on patterns or absence of information

## Rules

1. **Never invent.** If it's not in the source material, don't extract it. Note it as a gap.
2. **Prefer under-extraction to over-extraction.** It's better to flag a gap than to guess wrong. The interview will fill gaps.
3. **Preserve source language.** Use the words people actually used, not TML jargon. If they said "the sales team," write "the sales team" — don't write "Domain: Revenue Generation" unless they said that.
4. **Track provenance from the start.** Every candidate must cite where it came from. This becomes the foundation of the changelog.
5. **Separate fact from inference.** High-confidence candidates are facts. Low-confidence candidates are inferences. Label them honestly.
6. **Don't flatten complexity.** If something doesn't fit neatly into one primitive, say so. The interview will resolve ambiguity.

## Handoff

The output of this skill feeds directly into `tml-interview` as a warm start. The interview agent receives the extraction results and uses them to ask targeted confirmation and gap-filling questions instead of starting from zero.
