# Orchestration Pattern Card: Confidence-Gated Fallback

This example is generic and illustrative. It does not describe a real production system.

## 1. Pattern Summary

| Field | Value |
|---|---|
| Pattern name | Confidence-Gated Fallback |
| Pattern family | validation / failure-handling |
| Primary use case | contain low-confidence agent outputs and route them to fallback or human review |
| Risk level | Medium |
| Human oversight required | Conditional |

## 2. Problem

A primary agent may produce an output that appears plausible but has low confidence, incomplete evidence, or unresolved policy risk. If the workflow accepts that output automatically, users may receive unreliable or unsafe recommendations.

## 3. When to Use

Use this pattern when:

- agent outputs vary in confidence or evidence quality
- low-confidence outputs should not be accepted automatically
- a simpler fallback path or human review queue exists
- the system needs traceable decisions for why an output was accepted, retried, or escalated

Do not use this pattern when:

- the task is deterministic and does not need model judgment
- there is no meaningful fallback or escalation path
- confidence signals are not calibrated or are known to be unreliable

## 4. Agents and Roles

| Agent | Role | Input | Output | Owner |
|---|---|---|---|---|
| Orchestrator | orchestrator | task request | route and control decision | Platform owner |
| Primary Agent | executor | task request and context | proposed answer or action | Model owner |
| Validator Agent | validator | proposed output and acceptance criteria | confidence and pass/fail assessment | Quality owner |
| Fallback Agent | fallback | task request and safe reduced-scope rules | conservative fallback output | Platform owner |
| Human Reviewer | human oversight | flagged case and trace | approve, reject, or correct | Operations owner |

## 5. Control Flow

```text
Task request
  -> Orchestrator assigns Primary Agent
  -> Primary Agent produces output
  -> Validator Agent scores confidence and policy fit
  -> If accepted: return output
  -> If low confidence and retry budget remains: retry Primary Agent
  -> If retry fails and fallback available: use Fallback Agent
  -> If fallback fails or risk is high: escalate to Human Reviewer
```

## 6. Decision Rules

| Rule | Condition | Action |
|---|---|---|
| Accept | confidence >= 0.85 and no policy flags | return primary output |
| Retry | confidence between 0.60 and 0.84 and retry_count < 1 | retry primary agent once |
| Fallback | confidence < 0.60 or retry fails | use fallback agent |
| Escalate | high-risk category, policy flag, or fallback failure | route to human reviewer |

## 7. Failure Modes and Controls

| Failure mode | Detection signal | Control | Escalation trigger |
|---|---|---|---|
| primary output is plausible but wrong | validator confidence below threshold | retry or fallback | repeated low confidence |
| fallback output is too generic | fallback quality flag | human review | fallback quality below threshold |
| confidence score is misleading | periodic eval drift | recalibrate validator thresholds | repeated false accepts |
| high-risk task bypasses review | policy flag missed | deterministic high-risk rules | any missed high-risk escalation |

## 8. Logging Requirements

Minimum trace fields:

- task ID
- agent ID
- input summary
- output summary
- validator confidence
- decision rule applied
- retry count
- fallback used
- escalation decision
- timestamp

## 9. Evaluation Criteria

| Criterion | Target | Evidence |
|---|---|---|
| task completion | >= 95% accepted or safely escalated | benchmark scenario report |
| failure containment | 0 unhandled low-confidence outputs | decision log audit |
| escalation correctness | 100% for high-risk scenarios | red-team scenario pack |
| latency or cost | within configured SLA after at most one retry | run telemetry |

## 10. Related Patterns

- Validator Agent
- Retry with Backoff
- Fallback Agent
- Human Escalation
- Circuit Breaker

## 11. Implementation Notes

Keep the retry budget bounded. Unbounded retries hide system weakness, increase latency, and make audit trails harder to interpret.

Confidence thresholds should be treated as operational controls, not universal truths. They should be tuned using evaluation evidence from `agent-eval` and tested through executable scenarios in `agent-simulator`.
