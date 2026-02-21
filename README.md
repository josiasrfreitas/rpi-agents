# RPI — Research, Plan, Implement

Agent-assisted development pipeline with isolated responsibilities and artifact-only communication.

## The problem

Language models operate within a finite context window. The more irrelevant information enters that window, the worse the output gets. Long conversations with coding agents degrade progressively — the model forgets constraints, repeats mistakes and loses coherence.

## The approach

Split the work into specialized stages where each agent receives only the artifact it needs to do its job. No conversation history, no output from other agents, no accumulated context. Controlled context produces controlled results.

## Concepts

**Artifacts.** Agents do not talk to each other. All information between stages is a structured document that one stage produces and the next one consumes. If something is not in the artifact, it does not exist for the next agent. This eliminates dependency on conversation context, model memory and whoever executed the step.

**Single responsibility.** Each agent does one thing. Research does not suggest solutions. Plan does not write code. Implement does not replan. When an agent tries to do more than it should, the quality of everything it does drops.

**Adversarial review.** No artifact moves forward without being criticized by independent models. The goal is not consensus — it is finding gaps. If three different models find no relevant issues, confidence in the artifact is high enough to proceed.

## Pipeline

| Step | Action | Owner | Artifact |
|------|--------|-------|----------|
| 0 | Define scope | Human | — |
| 1 | Investigate codebase | Agent | `research-summary.md` |
| 2 | Challenge research | Adversarial models | — |
| 3 | Create plan | Agent | `implementation-plan.md` |
| 4 | Challenge plan | Adversarial models | — |
| 5 | Finalize plan | Human | — |
| 6 | Implement | Coding agent | `implementation-report.md` |
| 7 | Audit implementation | Adversarial models | `review-report.md` |
| 8 | Manual validation | Human | — |

Three moments are exclusively human: defining scope, making the final decision and validating the result.

## Agents

| Agent | Purpose |
|-------|---------|
| [01-research-agent.md](01-research-agent.md) | Map the current state of code relevant to the change |
| [02-plan-agent.md](02-plan-agent.md) | Transform validated facts into an execution plan |
| [03-implement-agent.md](03-implement-agent.md) | Execute the plan exactly as written |
| [04-review-agent.md](04-review-agent.md) | Audit conformity between plan and implementation |
| [05-decision-record-agent.md](05-decision-record-agent.md) | Record architectural decisions in MADR format |

## Artifact storage

All artifacts for a task live in `.rpi/<task-id>/` at the target project root:

```
.rpi/<task-id>/
├── research-summary.md
├── implementation-plan.md
├── implementation-report.md
├── review-report.md
└── adr-<task-id>.md           (when applicable)
```

Code shows what changed. Artifacts show why.
