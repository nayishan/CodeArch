---
name: task-orchestration
description: Intake, decompose, assign, coordinate, and close issue-driven bug-fixing work across multiple agents.
---

# Skill: Task Orchestration

## When to use

Use when a new issue enters the system, an existing investigation needs reassignment or reprioritization, a blocker needs coordination, or repair/review work must be scheduled.

## Required inputs

- Issue statement and available expected-behavior source.
- Repository/base commit and environment constraints.
- Available agents and their current workload/capabilities.
- Current Architect diagnosis, if available.
- Time, tool, token, or execution constraints, if known.

If critical context is missing, request it or create a bounded intake task. Do not invent repository facts.

## Procedure

### 1. Establish the task envelope

Create or update a canonical task board and stable run ID before dispatch. Record the issue and symptom, expected-behavior source, repository and submission revisions, task/data version, environment constraints, edit restrictions, available resources, and stage completion criteria. Separate issue status from task status; mark unknown provenance unavailable rather than guessing.

### 2. Obtain technical decomposition

Ask Architect for:
- the current bug-centric model;
- leading hypotheses and alternatives;
- likely investigation boundaries;
- cross-entity dependencies;
- checks that distinguish competing explanations;
- which work can safely proceed in parallel.

Do not independently invent technical partitions from file names alone.

### 3. Create work packages and register assignments

A useful task has a stable task ID and run ID, one objective, bounded scope, named owner, explicit input artifacts and deliverables, dependencies, constraints, acceptance criteria, and completion condition.

Register each assignment in the canonical board before dispatch. Track at minimum: `task_id`, `run_id`, `issue`, `owner`, `type`, `objective`, `scope`, `status`, `priority`, `dependencies`, `blocked_by`, `acceptance_criteria`, `input_artifacts`, `deliverable`, `evidence`, `next_action`, and `updated_at`. Add fields when real work requires them; do not force every communication to carry every field.

Partition by meaningful behavior, Entity, execution path, or hypothesis—not automatically by file or directory.

Avoid:
- two Workers unknowingly changing the same code;
- separate tasks that require incompatible interpretations of one Relationship;
- tasks so broad that no one can report a decisive result;
- tasks so small that coordination costs exceed the work.

### 4. Decide parallelism

Parallelize when tasks have sufficiently independent evidence paths or code areas.

Keep work sequential when:
- one result determines the next useful investigation;
- shared mutable code creates collision risk;
- multiple tasks depend on one unresolved semantic definition;
- a discriminating experiment should be run before broader exploration.

Architect recommends technical dependencies; Project Manager makes the actual allocation decision.

### 5. Assign review deliberately

For important diagnosis and repair claims, assign a Checker with:
- the original issue and relevant revision;
- the claim or artifact to review;
- access to underlying evidence;
- an explicit request for independent inspection;
- expected review output and deadline.

Do not ask Checker merely to “confirm” the Worker.

### 6. Manage progress and changes

On each update, compare the report with its deliverable and acceptance criteria; register the delivery, sender, recipient, artifact type/revision, evidence location, status, and next action; identify blockers; decide whether to continue, narrow, expand, split, or cancel; append status, scope, model, authorization, review, and rework changes to history; and notify affected agents.

When Worker evidence invalidates Architect's model, route it to Architect for reconciliation. Do not suppress it to preserve the original plan.

### 7. Authorize repair

Authorize implementation only when:
- the target behavior and acceptance criteria are explicit;
- the technical basis is adequate for the risk of the change;
- the allowed scope is clear;
- the Worker knows whether experiments or edits are permitted.

If diagnosis remains uncertain, authorize a narrow diagnostic change or experiment only when that is the intended objective.

### 8. Preserve provenance and close the coordination loop

For every execution, preserve Git and submission/config revisions, task/data version, model/harness when known, exact commands, timing, tool usage, results, traces, and patches. Store them in the canonical run record; mark unavailable values explicitly.

Close only after required Worker deliverables are registered; Checker findings are resolved or accepted as limitations; tests and skipped checks are recorded; patch reference and changed files are known; risks, provenance, and handoff history are preserved; and Evaluator can access process records when evaluation is required.

## Decision rules

- **Blocked**: a concrete dependency or missing input prevents progress. Assign an unblock action.
- **Needs revision**: the deliverable exists but fails its stated acceptance criteria. Return specific findings.
- **Completed**: the task deliverable is supplied; this does not imply the issue is fixed.
- **Issue resolved**: only use when the technical acceptance criteria are met within the stated validation scope.

## Deliverable

Maintain one canonical, inspectable task board and delivery register per issue/run, plus append-only status and handoff history. Use YAML, JSON, Markdown table, or another format readable by available agents. Keep core fields stable and extend the record when real work exposes a need. Preserve it where subsequent agents and Evaluator can read it. Keep technical diagnosis in Architect's model rather than duplicating it as managerial prose.

## Stop / escalate

Escalate to Architect for unresolved technical-model conflicts; to Checker for independent verification; and to the appropriate competition/operator authority for missing permissions, unavailable infrastructure, or policy constraints. Do not resolve technical uncertainty by managerial fiat.
