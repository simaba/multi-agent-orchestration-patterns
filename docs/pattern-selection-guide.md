# Pattern Selection Guide

Choose the orchestration pattern based on the structure of the task.

## Use sequential when
- each stage depends on the prior stage
- the workflow is easy to decompose into ordered steps

## Use parallel when
- multiple independent analyses can happen at once
- you want breadth or comparison across outputs

## Use feedback loop when
- output quality must be reviewed before acceptance
- revision cycles are acceptable and bounded

## Use fallback when
- failure or low confidence must trigger a safer path
- the primary flow is not reliable enough on its own

## Design rule
Pick the simplest pattern that reliably fits the use case.
