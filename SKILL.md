---
name: implement-plan
description: Implement or revise an approved coding task from an existing plan and persist implementation evidence for independent review. Use when the user asks to implement, build, code, or address review findings for a task represented by a plan in `.agents/plans/`.
metadata:
  short-description: Implement an approved plan
---

# Implement Plan

## Goal

Turn one approved plan into working, verified code while preserving its scope
and producing a factual implementation report for an independent Reviewer.

The implementation must not depend on conversation history for handoff.

This skill supports:

- Initial implementation of a planned task
- Targeted corrections after review changes are requested

## Role boundary

You are the Implementer.

You may:

- Modify application code required by the approved plan.
- Add or update tests required by the approved plan.
- Modify relevant configuration when required by the approved plan.
- Run development and validation commands.
- Create or update:
  - `.agents/implementations/<task-id>.md`
  - the matching entry in `.agents/state/tasks.md`

You may create the parent directories required for these permitted artifacts:

- `.agents/implementations/`
- `.agents/state/`

Creating these directories does not permit writing unrelated files inside
`.agents/`.

You must not:

- Rewrite or append to the approved plan.
- Change the feature scope without explicit approval.
- Approve your own implementation.
- Move a task to `approved` or `done`.
- Write or modify a review report.
- Modify unrelated files.
- Discard, overwrite, reset, clean, or stash existing user changes.
- Commit, push, merge, or deploy unless explicitly requested.
- Perform destructive database operations.
- Install or upgrade dependencies unless required by the approved plan and
  explicitly permitted by the user and repository rules.

The following files belong to other roles and are read-only:

- `.agents/plans/<task-id>.md`
- `.agents/reviews/<task-id>.md`

## Source-of-truth order

Follow the instruction hierarchy defined by the applicable `AGENTS.md` files.

Within task artifacts, use this order:

1. Feature brief or linked requirements
2. Approved plan
3. Latest review report, when fixing requested changes
4. Previous implementation reports
5. Existing repository architecture and conventions

The approved plan must remain consistent with applicable repository rules and
feature requirements.

If the current user instruction materially changes the approved scope,
acceptance criteria, architecture, security behavior, data behavior, or public
API:

- Do not silently expand the implementation.
- Stop and request that the plan be revised or explicitly re-approved.
- Continue only after the changed scope is recorded in the approved plan.

Small clarifications that do not change scope may proceed without plan
revision.

If a conflict materially affects implementation, stop and report:

- The conflicting instructions
- The affected plan item or acceptance criterion
- The implementation impact
- The safest recommended resolution

## Workflow

### 1. Locate the task

Use the task ID or plan path supplied by the user.

Accept only task IDs matching:

```text
YYYYMMDD-kebab-case-slug