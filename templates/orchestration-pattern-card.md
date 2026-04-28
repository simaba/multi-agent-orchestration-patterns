# Orchestration Pattern Card

Use this template to document a reusable multi-agent orchestration pattern.

## 1. Pattern Summary

| Field | Value |
|---|---|
| Pattern name | `[TBD]` |
| Pattern family | `[routing / delegation / validation / failure-handling]` |
| Primary use case | `[TBD]` |
| Risk level | `[low / medium / high]` |
| Human oversight required | `[yes / no / conditional]` |

## 2. Problem

Describe the orchestration problem this pattern solves.

`[TBD]`

## 3. When to Use

Use this pattern when:

- `[TBD]`
- `[TBD]`

Do not use this pattern when:

- `[TBD]`
- `[TBD]`

## 4. Agents and Roles

| Agent | Role | Input | Output | Owner |
|---|---|---|---|---|
| `[agent-name]` | `[orchestrator / executor / validator / monitor / fallback]` | `[TBD]` | `[TBD]` | `[TBD]` |

## 5. Control Flow

```text
[step 1] -> [step 2] -> [decision] -> [retry / fallback / escalate / accept]
```

## 6. Decision Rules

| Rule | Condition | Action |
|---|---|---|
| `[TBD]` | `[TBD]` | `[TBD]` |

## 7. Failure Modes and Controls

| Failure mode | Detection signal | Control | Escalation trigger |
|---|---|---|---|
| `[TBD]` | `[TBD]` | `[TBD]` | `[TBD]` |

## 8. Logging Requirements

Minimum trace fields:

- task ID
- agent ID
- input summary
- output summary
- decision rule applied
- retry count
- fallback used
- escalation decision
- timestamp

## 9. Evaluation Criteria

| Criterion | Target | Evidence |
|---|---|---|
| task completion | `[TBD]` | `[TBD]` |
| failure containment | `[TBD]` | `[TBD]` |
| escalation correctness | `[TBD]` | `[TBD]` |
| latency or cost | `[TBD]` | `[TBD]` |

## 10. Related Patterns

- `[TBD]`

## 11. Implementation Notes

`[TBD]`
