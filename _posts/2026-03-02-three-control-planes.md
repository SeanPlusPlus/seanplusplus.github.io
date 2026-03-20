---
layout: post
title: "Three Control Planes for Agentic Systems"
date: 2026-03-02
---

How control changes when systems start making their own decisions.

Software used to be predictable. A request came in, a response went out, and the path between them was fixed. Agents are not like that. They explore. They combine tools in ways no one anticipated. They act on their own judgment. The old controls do not work when the system decides for itself. Three new control planes do: observability, evaluation, and governance.

## Observability

**Observability is about understanding.**

Agentic systems do not replace traditional observability. They extend it. An agent is still software executing control flow, but that control flow is now probabilistic, branching, and stateful in ways that traditional request/response systems were not.

In traditional systems, observability is well understood. Logs, metrics, and traces describe what happened inside a service. Observability platforms ingest this telemetry and correlate it across hosts, services, and requests to answer questions about performance, reliability, and failure modes.

That foundation does not go away with agents. CPU usage, memory pressure, latency, retries, and error rates still matter. These platforms remain a core system of record for the operational health of the application running the agent.

What agents introduce is an additional layer of complexity: the *decision layer*. The most important failures in agentic systems are often not infrastructure failures, but decision failures.

In a traditional web service, the observable control flow is usually simple:

*request → handler → database → response*

An agent extends this into a richer execution loop:

*input → retrieval → planning → tool selection → tool execution → state update → termination*

Each arrow is still code. Each transition is still an instrumentation boundary. But the meaning of those transitions matters in a way that generic logs and traces alone cannot fully capture.

Observability answers questions like: what path did the agent take, what information did it see, which tools did it invoke, and where did it stop? It makes the internal behavior of the agent visible so it can be reasoned about, debugged, and improved.

## Evaluation

**Evaluation is about judgment.**

It answers a different question: given what the agent produced, was this acceptable, correct, aligned, or useful?

Unlike observability, evaluation is normative. It encodes expectations about behavior, not measurements of the system itself. There is no universally correct evaluation, only evaluations that reflect a particular set of goals and values.

Measurement theory helps make this explicit. Every evaluation defines a *construct* (what we claim to be measuring) and an *instrument* (how we score it). Two properties matter most: **reliability** and **validity**.

Reliability is about consistency. If the same output is scored multiple times, do we get the same result? This includes agreement between different graders, stability across repeated runs, and sensitivity to small changes in prompts or context. A measurement that is not repeatable cannot be trusted, regardless of how sophisticated it appears.

In a system with both a grading agent and a human grader, reliability shows up as agreement. Do multiple runs of the grading agent produce similar scores? Do humans agree with each other when grading the same output? Does the grading agent align with human judgment often enough to be useful? Reliability can be estimated statistically, but it always reflects how stable the evaluation process is in practice.

Validity is about correctness of intent. It asks whether the evaluation is actually measuring the behavior it claims to measure. A scoring function can be highly reliable and still wrong, consistently rewarding verbosity instead of reasoning, or confidence instead of truth.

Validity cannot be computed directly. It is established through argument, evidence, and iteration. Does the score change when the underlying behavior meaningfully changes? Does it correlate with outcomes we care about? Does it fail in predictable and understandable ways?

In a grading-agent-plus-human setup, validity is the harder problem. Agreement alone is not enough. Humans and agents can agree on the wrong thing. Validity requires humans to define what "good" means, inspect failures, refine rubrics, and decide whether the evaluation is incentivizing the behavior the system was designed to produce.

In practice, evaluation begins as code. Teams write assertions, heuristics, classifiers, and comparisons that score outputs. Because agent behavior is stochastic, evaluation in production systems is rarely binary. It is probabilistic, sampled, and trend-driven.

Evaluation does not explain behavior on its own. When an evaluation fails, observability is required to understand why it failed and what to change. When an evaluation succeeds, validity determines whether that success actually represents progress.

## Governance

**Governance is about control.**

It defines the boundaries within which the agent is allowed to operate, regardless of how clever or effective the agent might be.

Governance is not observability and not evaluation. It is constraint. It answers a different question: not what happened, and not whether it was good, but whether an action is allowed to happen at all.

In traditional systems, governance was largely implicit. Narrow control flow, static permissions, and fixed roles meant that many constraints were never exercised. The system simply could not do very much, so governance appeared simpler than it really was.

Agentic systems remove that safety blanket. An agent will explore the state space. It will combine tools creatively, probe boundaries, and attempt actions that were never explicitly anticipated by its designers.

As a result, governance must become explicit and continuously enforced at runtime. Instead of relying on tests that run before deployment, governance acts like a set of invariant checks that run on every step. If a rule matters, violating it must fail deterministically.

In this sense, governance resembles unit testing turned inside out. Tests assert what should succeed. Governance asserts what must never succeed. It enforces impossibility, not correctness.

Effective governance lives in capability boundaries, policy engines, sandboxed execution, approval gates, and budget limits. These controls sit between the agent and the world, shaping what actions are possible no matter how the agent reasons.

---

> Observability makes agent behavior visible, evaluation makes it measurable, and governance makes it safe.
