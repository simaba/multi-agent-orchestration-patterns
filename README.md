# Multi-Agent Orchestration Patterns

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)
[![Discussions](https://img.shields.io/badge/Discussions-Join-7289da?style=flat-square&logo=github)](https://github.com/simaba/agent-orchestration/discussions)

A catalog of design patterns for orchestrating multiple AI agents, covering routing, delegation, validation, and failure handling in LLM-based multi-agent pipelines.

---

## Choose this repo when

Use this repository when you need **control-flow patterns** for multi-agent systems:

- how work is routed
- how subtasks are delegated
- how outputs are validated
- how retries, fallbacks, and escalation are structured

Do **not** start here if you need the broader oversight model. Use [`multi-agent-governance`](https://github.com/simaba/multi-agent-governance).

Do **not** start here if you need evaluation criteria and pass/fail dimensions. Use [`agent-eval`](https://github.com/simaba/agent-eval).

Do **not** start here if you want runnable behavior. Use [`agent-simulator`](https://github.com/simaba/agent-simulator).

---

## Pattern catalog

### Routing patterns

| Pattern | When to Use | Description |
|---|---|---|
| **Classifier Router** | Multiple specialized agents | A classifier agent routes tasks to the most appropriate specialist |
| **Capability-Based Routing** | Agents with declared capabilities | Route tasks based on declared capabilities and task requirements |
| **Load-Balanced Round Robin** | Identical agent instances | Distribute tasks across replicas for throughput |
| **Priority Queue** | Mixed urgency tasks | Route high-priority tasks first and batch low-priority work |

### Delegation patterns

| Pattern | When to Use | Description |
|---|---|---|
| **Hierarchical Delegation** | Complex decomposable tasks | Orchestrator breaks tasks into subtasks and delegates execution |
| **Parallel Fan-Out** | Independent subtasks | Execute multiple subtasks in parallel and aggregate results |
| **Sequential Pipeline** | Order-dependent tasks | Pass outputs from one agent to the next in a fixed sequence |
| **Conditional Branch** | Decision-dependent workflows | Route to different agents based on intermediate results |

### Validation patterns

| Pattern | When to Use | Description |
|---|---|---|
| **Validator Agent** | High-stakes outputs | Separate agent reviews and approves outputs before use |
| **Majority Vote** | Uncertain tasks | Run the same task across N agents and accept the majority result |
| **Cross-Check** | Critical calculations | Two independent agents solve the same problem and divergence is flagged |
| **Confidence Gate** | Variable certainty | Only accept outputs above a threshold and escalate the rest |

### Failure-handling patterns

| Pattern | When to Use | Description |
|---|---|---|
| **Retry with Backoff** | Transient failures | Retry failed tasks with exponential backoff before escalation |
| **Fallback Agent** | Agent unavailability | Switch to a simpler fallback agent when the primary fails |
| **Human Escalation** | Unresolvable failures | Route to a human when retries and fallbacks are exhausted |
| **Circuit Breaker** | Cascading failures | Stop sending tasks to a failing agent until health checks recover |
| **Poison Pill Isolation** | Malformed inputs | Quarantine repeatedly failing inputs for review |

---

## Implementation examples

### Hierarchical delegation (Python pseudocode)

```python
from dataclasses import dataclass
from typing import Any

@dataclass
class Task:
    description: str
    priority: str  # high | medium | low
    requires_human_review: bool = False

class OrchestratorAgent:
    def __init__(self, executor_agents, validator_agent):
        self.executors = executor_agents
        self.validator = validator_agent

    def execute(self, task: Task) -> dict[str, Any]:
        executor = self._select_executor(task)
        result = executor.run(task)

        if task.priority == "high" or task.requires_human_review:
            validation = self.validator.validate(task, result)
            if not validation.approved:
                return self._escalate_to_human(task, result, validation.reason)

        return result

    def _select_executor(self, task):
        return self.executors[0]

    def _escalate_to_human(self, task, result, reason):
        return {"status": "pending_human_review", "reason": reason}
```

### Majority vote (Python pseudocode)

```python
from collections import Counter

def majority_vote(agents, task, n_votes=3):
    """Run task across N agents and return the majority answer."""
    responses = [agent.run(task) for agent in agents[:n_votes]]
    vote_counts = Counter(r["answer"] for r in responses)
    winner, count = vote_counts.most_common(1)[0]

    confidence = count / n_votes
    if confidence < 0.67:
        return {"answer": winner, "confidence": confidence, "escalate": True}

    return {"answer": winner, "confidence": confidence, "escalate": False}
```

---

## Governance considerations

For each pattern, document:

- **who owns the orchestrator**
- **what gets logged**
- **where human oversight is triggered**
- **how failure is detected and handled**

See [`multi-agent-governance`](https://github.com/simaba/multi-agent-governance) for the companion governance framework.

---

## Related repositories

| Repository | What it adds |
|---|---|
| [`multi-agent-governance`](https://github.com/simaba/multi-agent-governance) | oversight model, trust tiers, accountability structure |
| [`agent-eval`](https://github.com/simaba/agent-eval) | measurable evaluation dimensions and scenarios |
| [`agent-simulator`](https://github.com/simaba/agent-simulator) | runnable implementation of bounded agent workflows |
| [`lean-ai-ops`](https://github.com/simaba/lean-ai-ops) | applied workflow automation in a separate domain |

*Maintained by [Sima Bagheri](https://github.com/simaba) · Connect on [LinkedIn](https://www.linkedin.com/in/simaba/)*
