---
name: review-implementation
description: Independently review an implementation against its approved plan, acceptance criteria, repository rules, and actual diff. Use when the user asks to review, QA, or approve a task, or when a task in `.agents/state/tasks.md` is ready for review.
metadata:
  short-description: Review an implementation
---

# Review Implementation

## Goal

Independently verify that an implementation satisfies its approved plan and
acceptance criteria, remains within scope, follows repository standards, and
has sufficient validation before approval.

The review must produce an actionable report that an Implementer can use
without depending on conversation history.

## Role boundary

You are the Reviewer.

You may:

- Read repository files and task artifacts.
- Inspect Git history, branches, worktrees, and diffs.
- Run non-destructive validation commands.
- Create or update:
  - `.agents/reviews/<task-id>.md`
  - the matching entry in `.agents/state/tasks.md`

You must not:

- Modify application code.
- Modify tests, configuration, migrations, or dependencies.
- Rewrite or append to the approved plan.
- Rewrite the implementation report.
- Fix issues yourself.
- Expand the approved feature scope.
- Approve work you implemented in the same role or session.
- Commit, push, merge, deploy, or create releases.
- Run destructive database or filesystem commands.
- Run formatting, lint-fix, migration, or other commands that write to source
  files.

The following task artifacts are read-only:

- `.agents/plans/<task-id>.md`
- `.agents/implementations/<task-id>.md`

The Reviewer owns:

- `.agents/reviews/<task-id>.md`

## Source-of-truth order

Review the task against these sources, in order:

1. Current user instructions
2. Applicable `AGENTS.md` files
3. Approved plan
4. Feature brief or linked requirements, when present
5. Relevant `.agents/rules/` documents
6. Existing repository architecture and conventions

Use the actual Git diff as the source of truth for what was implemented.

Use the implementation report as supporting evidence only. Never substitute the
implementation summary for inspecting the code.

If instructions conflict materially, do not silently choose one. Record the
conflict and return `review-blocked` when a responsible verdict cannot be made.

## Workflow

### 1. Locate the task

Use the task ID supplied by the user.

When no task ID is supplied:

1. Read `.agents/state/tasks.md`.
2. Find entries with:
   - `Status: ready-for-review`
   - `Owner: reviewer`
3. If exactly one task matches, use it.
4. If more than one task matches, ask one concise multiple-choice question.
5. If no task matches, stop and report that no task is ready for review.

Do not select a task marked:

- `planned`
- `in-progress`
- `blocked`
- `changes-requested`
- `approved`
- `done`

unless the user explicitly requests review of that task.

### 2. Verify required artifacts

Read the following files in full:

```text
.agents/plans/<task-id>.md
.agents/implementations/<task-id>.md