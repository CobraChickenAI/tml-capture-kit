# Schema: hygiene-report

Output of `tml-hygiene`. The final quality gate before sharing.

## Structure

```markdown
# Hygiene Report

**Status:** PASS | PASS WITH FIXES | BLOCKED
**Date:** [ISO 8601]
**Artifacts reviewed:** [List]

## Missing Inputs

- [Artifact name] — [Why it's needed]

If any required artifact is missing, status is BLOCKED.

## Findings

### [Severity: Critical | Warning | Note] — [Issue title]
- **Where:** [Which artifact, which section]
- **Why it matters:** [What breaks or misleads if unfixed]
- **Fix:** [Specific action to resolve]

[Repeat for each finding]

## Check Results

### 1. Artifact Completeness
- [ ] Source registry exists with all sources identified
- [ ] Map has all required sections (scope, what we do, who's involved, rules, connections, views, provenance)
- [ ] Inferences document distinguishes stated vs. inferred vs. gap with confidence levels
- [ ] Changelog has source references and status for every element
- [ ] So What has all five analysis dimensions plus single starting point
- [ ] Agent Brief has operating context, capabilities, role, policies, systems, provenance requirements

### 2. Cross-Artifact Consistency
- [ ] Names match across all artifacts
- [ ] Policies in map match those in inferences, changelog, and agent brief
- [ ] So What findings reference actual map elements
- [ ] Agent Brief permissions align with map policies and archetypes

### 3. Provenance Quality
- [ ] All artifacts reference source IDs from the registry
- [ ] Every major element has a source path in the changelog
- [ ] Inferences document distinguishes stated vs. inferred
- [ ] Low-confidence items are explicit and reviewable
- [ ] No unsourced claims

### 4. Actionability
- [ ] Quick wins include action, effort, and impact
- [ ] AI leverage points define full operating contract (archetype, policies, read/write, boundaries)
- [ ] Recommended starting point is singular, specific, and feasible this week

### 5. Language Hygiene
- [ ] Plain language throughout — 7th-8th grade reading level
- [ ] No vague filler without specifics
- [ ] Uncertainty is explicit, not hidden

## Fast Fixes

Numbered, highest impact first.

1. [Fix description] — [Which artifact, which section]
2. [Fix description]

## Ready to Share

**Yes** | **No — [remaining items]**
```

## Rules

- If any required artifact is missing, the report is BLOCKED — do not proceed to findings.
- Severity levels: Critical (must fix before sharing), Warning (should fix), Note (nice to have).
- Fast Fixes are ordered by impact, not by severity.
