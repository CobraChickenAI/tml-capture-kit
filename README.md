# TML Capture Kit

Capture architecture from conversation. One flow, seven skills built on the open [Agent Skills](https://agentskills.io/specification) standard, a plain-language map anyone can read.

## What this does

You give it documents about how something works -- SOPs, wiki pages, meeting notes, whatever you have. It asks a few questions. You get back a complete architecture map with visible reasoning and actionable opportunities.

Total time for the human: about 20 minutes.

## The pipeline

```
CONTEXT    ──▶  INFORMED    ──▶  SURFACE     ──▶  THREE      ──▶  "SO       ──▶  QUALITY
DUMP            INTERVIEW        INFERENCES       ARTIFACTS       WHAT?"         GATE

tml-extract     tml-interview    tml-infer        tml-map         tml-so-what    tml-hygiene
10 min          10 min           auto             auto            auto           auto
```

| Stage | Skill | What it does |
|-------|-------|-------------|
| 1 | `tml-extract` | Pulls candidate primitives from unstructured documents |
| 2 | `tml-interview` | Confirms and refines candidates through conversation |
| 3 | `tml-infer` | Separates what was said from what was concluded |
| 4 | `tml-map` | Generates the Map, Changelog, and Agent Brief |
| 5 | `tml-so-what` | Surfaces opportunities, friction, and quick wins |
| 6 | `tml-hygiene` | Quality gate before sharing with stakeholders |

`tml-capture` is the orchestrator -- it runs the full pipeline as one seamless experience. Invoke it, not the sub-skills.

## What you get back

| Artifact | What it is | Who it's for |
|----------|-----------|-------------|
| The Map | Plain-language architecture -- what you do, who does it, what the rules are | Everyone |
| The Changelog | Where every element came from | Anyone asking "where did this come from?" |
| The Inferences | What the system concluded vs. what was stated | The person auditing the map |
| The Agent Brief | Operational instructions for AI agents | Any AI working within this scope |
| The So What? | Opportunities, friction points, quick wins | The person deciding what to do next |
| Hygiene Report | Quality check -- consistency, provenance, actionability | The person before they hit send |

## Install

These skills follow the open [Agent Skills specification](https://agentskills.io/specification) and work with any compatible agent.

Each skill directory is self-contained — SKILL.md plus its own `references/` with output schemas and templates.

**Claude Code**

```bash
git clone https://github.com/CobraChickenAI/tml-capture-kit.git
cp -r tml-capture-kit/skills/tml-* /path/to/your-project/.claude/skills/
```

**Codex**

```bash
npx skills add CobraChickenAI/tml-capture-kit
```

**Cursor / VS Code**

```bash
git clone https://github.com/CobraChickenAI/tml-capture-kit.git .cursor/skills/tml-capture-kit
```

Or download the repo as a zip and extract the `skills/` directory into your project.

## Data contracts

Every handoff between skills has a formal schema. Each skill carries the schemas for its own outputs in its `references/` directory. The orchestrator (`tml-capture`) holds all schemas and the pipeline definition (`pipeline.yaml`) — the machine-readable DAG that governs execution order, data flow, and human gates.

## Part of The Missing Layer

This kit is part of the [TML ecosystem](https://github.com/CobraChickenAI/themissinglayer). TML provides the primitives (Scope, Domain, Capability, Archetype, Policy, Connector, Binding, View, Provenance) that give the capture pipeline its structure.

## License

MIT. See [LICENSE](LICENSE).
