---
name: tml-hygiene
description: "Quality-control pass for TML outputs. Reviews Map, Inferences, Changelog, So What, and optional Agent Brief for consistency, provenance completeness, ambiguity, and actionability before sharing. Part of the TML capture pipeline — runs last, after tml-so-what. Final gate before sharing with stakeholders."
license: MIT
metadata:
  author: Michael Schartung
  repository: CobraChickenAI/tml-capture-kit
  version: "1.0.0"
  pipeline: tml-capture-pipeline
  pipeline-stage: "6"
  inputs: "the-map, the-changelog, the-agent-brief, inferences-document, so-what-analysis"
  outputs: hygiene-report
  schema-ref: references/hygiene-report.md
---

# TML Hygiene

Run this skill after a mapping session and before sharing outputs with stakeholders.

The goal is not to rewrite everything. The goal is to catch drift, contradictions, weak provenance, and vague recommendations while fixes are still cheap.

## When To Use

- After `tml-capture` completes
- Before sending outputs to a manager, DPM, or external partner
- During repo review, acceptance testing, and pilot rollouts

## Inputs

Provide:

- The Map
- The Inferences
- The Changelog
- The So What
- Optional Agent Brief

If any artifact is missing, flag it as a blocker.

## Checks

### 1. Artifact Completeness

Confirm all required sections are present and non-empty.

- Source registry exists (Source 1, Source 2, ...).
- Map has: scope summary, what we do, who is involved, rules, connectors/bindings, provenance section.
- Inferences has: stated vs inferred breakdown and confidence.
- Changelog has: source references and status per major element.
- So What has: visibility gaps, friction points, coherence risks, quick wins, AI leverage points, single recommended starting point.

### 2. Cross-Artifact Consistency

Find contradictions between documents.

- Names match across artifacts for scope/domains/capabilities.
- Policies in map match policy references in inferences/changelog.
- So What findings point to elements that actually exist in the map.
- Agent Brief permissions match map policies and archetypes.

### 3. Provenance Quality

Check whether claims trace to a source.

- Every artifact references source ids from the registry.
- Every major map element has a source path in changelog.
- Inference claims distinguish stated vs inferred.
- Low-confidence items are explicit and reviewable.
- No unsupported “fact” appears without a source or inference label.

### 4. Actionability

Reject generic recommendations.

- Quick wins include a concrete action, estimated effort, and expected impact.
- AI leverage points define operating archetype, policies, read/write boundaries, and explicit cannot-do constraints.
- Recommended starting point is singular, specific, and feasible this week.

### 5. Language Hygiene

Ensure outputs are clear for non-technical stakeholders.

- Plain language, short sentences, minimal jargon.
- No vague filler (“optimize,” “enhance,” “leverage synergies”) without specifics.
- Uncertainty is explicit, not hidden.

## Output

Return one hygiene report:

```markdown
# Hygiene Report: [Scope Name]

## Status
- PASS | PASS WITH FIXES | BLOCKED

## Findings
1. [Severity: High/Med/Low] [Issue]
   - Where: [artifact + section]
   - Why it matters: [impact]
   - Fix: [specific edit]

## Missing Inputs
- [any missing artifacts]

## Fast Fixes (Do Now)
1. [highest impact quick fix]
2. [next fix]
3. [next fix]

## Ready To Share
- Yes/No
- If no, what remains:
  - [list]
```

## Rules

1. Be direct and concrete.
2. Do not invent missing facts; request correction.
3. Prioritize trust issues first (provenance, contradictions, permissions).
4. Keep final recommendations implementation-ready.

---

*v1.0 — quality gate for release readiness and stakeholder confidence.*
