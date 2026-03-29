# Feedback Loop Pattern

A feedback loop pattern uses an evaluator or reviewer agent to inspect outputs and trigger limited revision cycles.

## Structure
An executor produces an output, an evaluator checks it, and the system either accepts it or sends it back for revision.

## Best use cases
- quality-sensitive drafting
- iterative refinement
- systems with explicit acceptance criteria

## Strengths
- improved quality control
- clearer acceptance checks
- useful for structured iteration

## Risks
- endless loops without retry limits
- latency growth
- over-correction if evaluator criteria are weak
