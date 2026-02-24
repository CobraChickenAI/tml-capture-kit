---
name: tml-so-what
description: "Analyze a TML architecture map and surface specific, grounded opportunities — where AI can help, where processes can improve, where gaps create risk. The 'So What?' that turns documentation into action. Part of the TML capture pipeline — runs after tml-map, before tml-hygiene."
license: MIT
metadata:
  author: Michael Schartung
  repository: CobraChickenAI/tml-capture-kit
  version: "1.0.0"
  pipeline: tml-capture-pipeline
  pipeline-stage: "5"
  inputs: "the-map, the-changelog"
  outputs: so-what-analysis
  schema-ref: references/so-what-analysis.md
---

# TML So What?

Takes an architecture map (from `tml-map`) and answers the question everyone asks after documentation: **"So what?"**

This skill analyzes the declared architecture and surfaces specific, grounded opportunities. Not generic advice. Not "you should use AI." Specific observations tied to specific parts of the map, with specific suggestions for what to do about them.

## When to Use

- After `tml-map` has produced the three artifacts
- As the final step in a `tml-capture` orchestration
- Standalone, against any existing architecture map
- Periodically, to re-analyze a map as conditions change

## The Analysis

### What You're Looking For

The analysis examines five dimensions of the map. For each dimension, surface only what's actually there — don't manufacture insights.

---

### 1. Visibility Gaps

**Question:** What's invisible that should be visible?

Look for:
- **Missing provenance.** Capabilities with no tracking. Actions with no audit trail. Decisions with no record.
- **Undeclared ownership.** Capabilities that don't clearly belong to a domain. Things that "everyone" owns (which means nobody does).
- **Implicit policies.** Rules that clearly exist in practice but aren't written down. "Everyone knows you have to..." = a policy that needs declaring.
- **Shadow capabilities.** Things the team does that aren't in the map. If the interview surfaced them as asides or afterthoughts, they're probably real and probably undeclared.

**Why it matters:** You can't improve what you can't see. Visibility gaps are the first place entropy hides.

**Output format:**
```markdown
#### Visibility Gaps

**[Gap name]**
Where: [which part of the map]
What's invisible: [what can't currently be seen]
Risk: [what could go wrong because of this]
Suggestion: [specific action to make it visible]
```

---

### 2. Friction Points

**Question:** Where does work get stuck, slow down, or require heroics?

Look for:
- **Manual handoffs** between capabilities or domains. Data copied from one system to another by hand.
- **Approval bottlenecks.** Policies that require human approval where the criteria are clear enough to automate or delegate.
- **Information re-gathering.** People asking for information that exists somewhere else in the system. Repeated context-setting.
- **Translation labor.** Converting data or information from one format to another manually.
- **Single points of failure.** Capabilities or knowledge that depend on one person.

**Why it matters:** Friction is where time and energy are wasted. It's also where AI has the most immediate, tangible impact — not replacing people, but removing the tedious parts.

**Output format:**
```markdown
#### Friction Points

**[Friction name]**
Where: [which capability, handoff, or process step]
What happens: [description of the friction]
Impact: [time wasted, errors introduced, frustration caused]
Suggestion: [specific action — could be AI, could be process change, could be just making something visible]
```

---

### 3. Coherence Risks

**Question:** Where is the architecture likely to break as things change?

Look for:
- **Duplicated truth.** The same information maintained in multiple places. If two systems need to agree, which one is authoritative?
- **Unconnected capabilities.** Things that should feed each other but don't have declared connectors or bindings.
- **Policy conflicts.** Rules that could contradict each other at scale.
- **Scope creep signals.** Capabilities that span multiple domains or serve unclear purposes.
- **Drift potential.** Areas where the declared architecture might not match reality in 6 months.

**Why it matters:** Coherence isn't a one-time achievement. It degrades over time. Identifying where it's most likely to degrade lets you build early warning systems.

**Output format:**
```markdown
#### Coherence Risks

**[Risk name]**
Where: [which part of the architecture]
What could break: [how coherence could degrade]
Likelihood: [high/medium/low — based on what you see in the map]
Suggestion: [specific action to prevent or detect this]
```

---

### 4. Quick Wins

**Question:** What could be improved THIS WEEK with minimal effort?

Look for:
- **Documentation that already exists but isn't accessible.** Making it findable is a quick win.
- **Simple automations.** Notifications, reminders, status updates that are currently manual.
- **Connections that just need wiring.** Two systems that could share data with a simple integration.
- **Policies that just need writing down.** Rules everyone follows but nobody documented.
- **Views that just need creating.** Dashboards, reports, or summaries that would give immediate visibility.

**Constraint:** A quick win must be achievable in less than one week by one person (or one agent) with no infrastructure changes. If it requires a project, it's not a quick win.

**Output format:**
```markdown
#### Quick Wins

**[Win name]**
Where: [which part of the map]
What to do: [specific, concrete action]
Effort: [hours, not days]
Impact: [what changes for the better]
```

---

### 5. AI Leverage Points

**Question:** Where can AI create the most value, given this specific architecture?

This is NOT "here are 10 ways AI can help your business." This is: given what we know about THIS system — its capabilities, actors, policies, connections — where does AI fit in a way that respects the architecture and actually helps?

Look for:
- **High-volume, pattern-based capabilities.** Things done repeatedly that follow clear rules.
- **Translation and summarization.** Capabilities that involve converting information from one form to another.
- **Monitoring and alerting.** Connectors that could be observed continuously instead of checked manually.
- **First-draft generation.** Capabilities where the output follows a template and human review adds the value.
- **Context-gathering.** Capabilities where 80% of the work is collecting information from multiple sources before making a decision.

**For each AI leverage point, specify:**
- **What archetype the AI would operate as** (from the map's existing archetypes, or suggest a new constrained one)
- **What policies bind it** (from the map's existing policies)
- **What it can read** (existing connectors)
- **What it can write** (existing bindings, or new ones needed)
- **What it cannot do** (explicit boundaries)

This makes the suggestion *architectural* — not just "use AI here" but "here's exactly how AI fits into the declared structure, with what permissions, under what constraints."

**Output format:**
```markdown
#### AI Leverage Points

**[Leverage point name]**
Capability: [which capability from the map]
What AI does: [specific task, in plain language]
Operating as: [archetype — existing or proposed]
Bound by: [which policies apply]
Reads from: [which connectors]
Writes to: [which bindings]
Cannot: [explicit boundaries]
Expected impact: [what changes — time saved, errors reduced, visibility gained]
Prerequisite: [what needs to be true first — data access, policy clarification, etc.]
```

---

## Output Format

Present the full analysis as a single document:

```markdown
# So What? Analysis: [Scope Name]

**Based on:** Architecture map dated [date]
**Analyzed:** [date]

---

## Summary

[3-5 sentences: the headline findings. What's the most important thing this analysis reveals? Lead with the highest-impact observation.]

---

## Visibility Gaps
[findings]

## Friction Points
[findings]

## Coherence Risks
[findings]

## Quick Wins
[findings]

## AI Leverage Points
[findings]

---

## Recommended Starting Point

If you could only do one thing from this analysis, do this:

**[The single highest-impact, most achievable action from everything above.]**

Why: [one sentence explaining why this is the place to start]

---

## What to Revisit

This analysis reflects the architecture as of [date]. Re-run this analysis when:
- A major process change occurs
- New systems are added or removed
- Team structure changes
- 90 days have passed (architecture drifts)

---

*Generated from TML architecture map. Analysis is grounded in declared structure — not generic advice.*
```

---

## Analysis Rules

1. **Be specific or be quiet.** Every finding must reference a specific part of the map. "You should improve communication" is banned. "The handoff between [Capability X] and [Capability Y] currently requires [person] to copy data manually from [System A] to [System B]" is useful.

2. **Don't oversell AI.** Not everything needs AI. Some problems need a spreadsheet. Some need a conversation. Some need a policy written down. Recommend what actually fits.

3. **Respect the architecture.** Suggestions must work within the declared structure or explicitly note what architectural changes they require. "Just add an AI agent" without specifying its archetype, policies, and connections is not a suggestion — it's a wish.

4. **Rank by impact and effort.** Lead with high-impact, low-effort items. The person reading this needs to know where to start.

5. **Use their language.** If the map says "the sales team," say "the sales team." Don't translate into consulting-speak.

6. **Acknowledge what's working.** Not everything is a problem. If the architecture reveals areas that are well-structured, say so. Credibility comes from honesty, not from finding problems everywhere.

7. **One recommended starting point.** The analysis must end with a single, clear "do this first." Decision fatigue is real. Give them the answer.

---

*v1.0 — The question that turns documentation into action.*
