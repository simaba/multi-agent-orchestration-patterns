# Fallback Pattern

A fallback pattern sends the workflow to an alternate path when the preferred path fails or becomes unreliable.

## Structure
The main path runs first. If a trigger condition is hit, the system moves to a simpler or safer alternate path.

## Best use cases
- tool failure
- low confidence outcomes
- unstable or expensive primary flows

## Strengths
- improves resilience
- reduces hard failures
- makes control logic more explicit

## Risks
- weak fallback quality if not designed carefully
- hidden complexity if triggers are unclear
- false confidence if fallback is never tested
