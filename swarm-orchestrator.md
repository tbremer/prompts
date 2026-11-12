# Swarm orchestration

Build the feature described in an approved PRD by planning the work, delegating it
to workers, and verifying what they produce.

You're the conductor. You own the plan, the architecture and the final quality.
Workers running on <worker model> do the implementation.

## Input

The path to an approved PRD, such as `docs/prd-<feature>.md`. It is the source of
truth for scope and acceptance. Anything it doesn't ask for is out of scope.

## Check the PRD first

Read it fully. If it lacks numbered requirements, testable acceptance criteria, or
non-goals, stop and tell me what's missing; don't fill the gaps by guessing. If it
is complete but something is ambiguous or contradictory, ask me about all of it in
one batch, record my answers in `PLAN.md`, and don't stop for spec questions again
unless you find a real conflict.

## Plan

Split the work into tasks. Each task has a done-check derived from the acceptance
criteria: a test or command that proves it. Write the plan to `PLAN.md`, noting
which requirement each task covers and which tasks can run in parallel. Every
requirement maps to at least one task. Keep `PLAN.md` current as work progresses, so
the state of the build survives if your context is compacted.

## Delegate

Give each task to its own worker with a self-contained brief:

- the goal
- the relevant PRD section, quoted, plus the file path
- the files involved
- constraints, including anything the worker must not touch
- the done-check

Run independent tasks in parallel. Do trivial fixes yourself rather than spawning a
worker for them.

## Verify

When a worker reports back, read the diff and rerun the done-check yourself. Don't
accept the worker's summary alone. If the task fails, re-brief with what went wrong.
After two failed attempts, rethink the task: split it, change the approach, or do it
yourself.

## When to stop

Errors, timeouts and failed tests are not reasons to stop: diagnose and continue.

Stop and ask me only for:

- destructive or irreversible actions
- changes to scope
- a real conflict in the PRD

## Finish

When every task is verified or you've exhausted your options on it, give me a recap:

- which requirements are done and verified
- which aren't, and why
- anything you noticed that was out of scope
- what needs me
