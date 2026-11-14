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

Record:
- issue identifier and symptom;
- expected behavior source, if known;
- repository revision and test/environment constraints;
- prohibited actions or edit restrictions;
- available time/resources;
- what “done” means for this stage.

Separate issue-resolution status from the status of individual tasks.

### 2. Obtain technical decomposition

Ask Architect for:
- the current bug-centric model;
- leading hypotheses and alternatives;
- likely investigation boundaries;
- cross-entity dependencies;
- checks that distinguish competing explanations;
- which work can safely proceed in parallel.

Do not independently invent technical partitions from file names alone.

### 3. Create work packages

A useful task has:
- one clear objective;
- a bounded but not artificially narrow scope;
- a named owner;
- explicit deliverables;
- dependencies and constraints;
- a completion condition.

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

On each update:
- compare reported work with the task deliverable;
- identify blockers and dependencies;
- decide whether to continue, narrow, expand, split, or cancel;
- record why scope or priority changed;
- notify affected agents.

When Worker evidence invalidates Architect's model, route the evidence to Architect for model reconciliation. Do not suppress it to preserve the original plan.

### 7. Authorize repair

Authorize implementation only when:
- the target behavior and acceptance criteria are explicit;
- the technical basis is adequate for the risk of the change;
- the allowed scope is clear;
- the Worker knows whether experiments or edits are permitted.

If diagnosis remains uncertain, authorize a narrow diagnostic change or experiment only when that is the intended objective.

### 8. Close the coordination loop

Close only after:
- required Worker deliverables are present;
- Checker findings are resolved or explicitly accepted as limitations;
- test results and skipped checks are recorded;
- patch reference and changed files are known;
- unresolved risks are visible;
- Evaluator has enough process data, when evaluation is required.

## Decision rules

- **Blocked**: a concrete dependency or missing input prevents progress. Assign an unblock action.
- **Needs revision**: the deliverable exists but fails its stated acceptance criteria. Return specific findings.
- **Completed**: the task deliverable is supplied; this does not imply the issue is fixed.
- **Issue resolved**: only use when the technical acceptance criteria are met within the stated validation scope.

## Deliverable

Maintain a concise task plan containing task ID, owner, objective, scope, dependencies, status, deliverable, and next action. Keep technical diagnosis in Architect's model rather than duplicating it as managerial prose.

## Stop / escalate

Escalate to Architect for unresolved technical-model conflicts; to Checker for independent verification; and to the appropriate competition/operator authority for missing permissions, unavailable infrastructure, or policy constraints. Do not resolve technical uncertainty by managerial fiat.
