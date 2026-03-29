# Multi-Agent Orchestration Patterns

A practical pattern library for structuring how agents interact in multi-agent AI systems.

## Why this repository exists

Many multi-agent systems are built as ad hoc workflows without a clear interaction structure.

This leads to:
- fragile coordination,
- unnecessary complexity,
- unclear fallback behavior,
- hard-to-debug control flow.

This repository provides reusable orchestration patterns that help builders choose the right interaction model instead of guessing.

## What this repository includes

- sequential agents
- parallel agents with aggregation
- feedback loop agents with evaluation
- fallback agents and alternative paths
- design guidance on when to use each pattern

## Who this is for

- builders of agent pipelines
- LLM application developers
- AI product and systems designers
- teams choosing between orchestration structures

## Repository structure

- `patterns/sequential.md`
- `patterns/parallel.md`
- `patterns/feedback-loop.md`
- `patterns/fallback.md`
- `docs/pattern-selection-guide.md`
- `diagrams/sequential-flow.mmd`
- `diagrams/parallel-flow.mmd`
- `examples/content-research-pipeline.md`

## Design principle

A good orchestration pattern reduces design ambiguity and makes the system easier to reason about before it is implemented.
