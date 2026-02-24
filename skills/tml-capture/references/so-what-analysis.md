# Schema: so-what-analysis

Output of `tml-so-what`. Turns the map from documentation into action.

## Structure

```markdown
# So What? Analysis

## Summary

[3-5 sentences. What the map reveals at a high level. What stands out.]

## Visibility Gaps

Findings where something important is invisible — missing provenance, undeclared ownership, implicit policies, shadow capabilities.

**[Finding name]**
- **Where:** [Which part of the map]
- **What:** [Specific description]
- **Risk:** [What could go wrong]
- **Suggestion:** [Concrete action]

## Friction Points

Findings where work is harder than it should be — manual handoffs, bottlenecks, re-gathering information, translation labor, single points of failure.

**[Finding name]**
- **Where:** [Which part of the map]
- **What:** [Specific description]
- **Impact:** [Time/effort/quality cost]
- **Suggestion:** [Concrete action]

## Coherence Risks

Findings where the architecture contradicts itself or drifts — duplicated truth, unconnected capabilities, policy conflicts, scope creep.

**[Finding name]**
- **Where:** [Which part of the map]
- **What:** [Specific description]
- **Risk:** [What breaks if this isn't addressed]
- **Suggestion:** [Concrete action]

## Quick Wins

Achievable in less than one week by one person or one agent. No infrastructure changes required.

**[Finding name]**
- **Action:** [What to do]
- **Effort:** [Estimate]
- **Impact:** [What improves]

## AI Leverage Points

Specific to THIS architecture. Not generic "you should use AI" advice.

**[Finding name]**
- **Archetype:** [What role the AI operates as — existing or new]
- **Policies that bind it:** [List]
- **Reads (connectors):** [What it accesses]
- **Writes (bindings):** [What it produces]
- **Cannot:** [Explicit boundaries]
- **Why here:** [What makes this a good fit for AI specifically]

## Recommended Starting Point

[One specific action. Singular. Feasible this week. Highest leverage.]

## What to Revisit

[When to re-run this analysis — e.g., after a reorg, after onboarding a new team, after deploying an AI agent]
```

## Rules

- Be specific or be silent. No generic advice.
- Every finding references a specific part of the map.
- Respect the declared architecture — suggest within it, or note what structural changes would be needed.
- Acknowledge what's working, not just problems.
- AI leverage points must define the full operating contract (archetype, policies, read/write, boundaries).
