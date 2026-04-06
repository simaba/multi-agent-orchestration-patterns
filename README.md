# Multi-Agent Orchestration Patterns

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)
[![Discussions](https://img.shields.io/badge/Discussions-Join-7289da?style=flat-square&logo=github)](https://github.com/simaba/multi-agent-orchestration-patterns/discussions)

A catalog of design patterns for orchestrating multiple AI agents — covering
routing, delegation, validation, and failure handling in LLM-based multi-agent pipelines.

---

## Pattern Catalog

### Routing Patterns

| Pattern | When to Use | Description |
|---|---|---|
| **Classifier Router** | Multiple specialized agents | A classifier agent routes tasks to the most appropriate specialist |
| **Capability-Based Routing** | Agents with declared capabilities | Route tasks based on agent capability declarations and task requirements |
| **Load-Balanced Round Robin** | Identical agent instances | Distribute tasks evenly across agent replicas for throughput |
| **Priority Queue** | Mixed urgency tasks | Route high-priority tasks to dedicated agents; batch low-priority |

### Delegation Patterns

| Pattern | When to Use | Description |
|---|---|---|
| **Hierarchical Delegation** | Complex decomposable tasks | Orchestrator breaks tasks into subtasks; delegates to executor agents |
| **Parallel Fan-Out** | Independent subtasks | Execute multiple subtasks in parallel; aggregate results |
| **Sequential Pipeline** | Order-dependent tasks | Pass outputs from one agent as inputs to the next in a fixed sequence |
| **Conditional Branch** | Decision-dependent workflows | Route to different agents based on intermediate results |

### Validation Patterns

| Pattern | When to Use | Description |
|---|---|---|
| **Validator Agent** | High-stakes outputs | A separate agent reviews and approves outputs before they are used |
| **Majority Vote** | Uncertain tasks | Run the same task across N agents; accept the majority result |
| **Cross-Check** | Critical calculations | Two independent agents solve the same problem; flag divergence |
| **Confidence Gate** | Variable certainty | Only accept outputs above a confidence threshold; escalate the rest |

### Failure Handling Patterns

| Pattern | When to Use | Description |
|---|---|---|
| **Retry with Backoff** | Transient agent failures | Retry failed tasks with exponential backoff before escalating |
| **Fallback Agent** | Agent unavailability | Switch to a simpler/cheaper fallback agent on primary failure |
| **Human Escalation** | Unresolvable agent failures | Route to a human when agent retries and fallbacks are exhausted |
| **Circuit Breaker** | Cascading failures | Stop sending tasks to a failing agent; restore after health check passes |
| **Poison Pill Isolation** | Malformed inputs | Quarantine tasks that cause repeated failures; flag for review |

---

## Implementation Examples

### Hierarchical Delegation (Python pseudocode)

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
        # 1. Select the best executor
        executor = self._select_executor(task)

        # 2. Delegate and get result
        result = executor.run(task)

        # 3. Validate if high-stakes
        if task.priority == "high" or task.requires_human_review:
            validation = self.validator.validate(task, result)
            if not validation.approved:
                return self._escalate_to_human(task, result, validation.reason)

        return result

    def _select_executor(self, task):
        # Route based on task type — extend with classifier logic
        return self.executors[0]

    def _escalate_to_human(self, task, result, reason):
        # Log escalation, notify on-call, return pending status
        return {"status": "pending_human_review", "reason": reason}
```

### Majority Vote (Python pseudocode)

```python
from collections import Counter

def majority_vote(agents, task, n_votes=3):
    """Run task across N agents and return the majority answer."""
    responses = [agent.run(task) for agent in agents[:n_votes]]
    vote_counts = Counter(r["answer"] for r in responses)
    winner, count = vote_counts.most_common(1)[0]

    confidence = count / n_votes
    if confidence < 0.67:  # No clear majority
        return {"answer": winner, "confidence": confidence, "escalate": True}

    return {"answer": winner, "confidence": confidence, "escalate": False}
```

---

## Governance Considerations

For each pattern, document:
- **Who owns the orchestrator** — the orchestrator is the accountability anchor
- **What gets logged** — at minimum: task ID, agent IDs involved, timestamps, outputs
- **Where human oversight is triggered** — define thresholds before deployment
- **How failure is detected and handled** — document the failure handling pattern used

See [multi-agent-governance-framework](https://github.com/simaba/multi-agent-governance-framework)
for the full governance framework applicable to systems using these patterns.

---

## Ecosystem

| Repository | Purpose |
|---|---|
| [multi-agent-governance-framework](https://github.com/simaba/multi-agent-governance-framework) | Governance framework for multi-agent systems |
| [agent-system-simulator](https://github.com/simaba/agent-system-simulator) | Simulate multi-agent behavior and failure modes |
| [ai-agent-evaluation-framework](https://github.com/simaba/ai-agent-evaluation-framework) | Evaluation metrics for AI agents |
| [LLM-powered-Lean-Six-Sigma](https://github.com/simaba/LLM-powered-Lean-Six-Sigma) | LLM agents applied to process improvement |

*Maintained by [Sima Bagheri](https://github.com/simaba) · Connect on [LinkedIn](https://www.linkedin.com/in/simabagheri)*
