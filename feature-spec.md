# Feature spec

Turn a feature idea, or an existing PRD, into an approved spec that someone else
could build from. This session produces a spec file and nothing else: write no
feature code.

## Input

Either a short description of the feature, or the path to an existing PRD.

## Before asking anything

Read the relevant parts of the repo, so you don't ask me things the code can answer.
Note the constraints the existing code puts on the feature.

## If there is no PRD: interview me

Ask questions in rounds of 3-5, most important first. For each question, give your
recommended answer and why, so I can say "yes" where I agree. Follow up when an
answer opens new questions or conflicts with something I said earlier.

Cover:

- the problem and who it's for
- what's in scope
- what's explicitly out of scope
- the main flows
- edge cases and failure behaviour
- constraints from the existing code
- how we'll know each requirement works

## If there is a PRD: review it

Read it fully and check it against the list above. Ask only about what is missing,
ambiguous, contradictory, or untestable, in the same rounds of 3-5 with recommended
answers. Don't re-ask what the PRD already settles.

## When to stop asking

Keep going until both are true:

- you can write a testable acceptance criterion for every requirement
- no open question remains whose answer would change the design

Don't stop early to be polite. Don't keep asking about details that wouldn't change
what gets built: decide those yourself and record them as assumptions.

## Output

Write the spec to `docs/prd-<feature>.md`, updating the existing file if there was
one. It always has these sections, in this order:

1. **Summary**: what the feature is and why, in a short paragraph.
2. **Requirements**: numbered (R1, R2, ...), one behaviour each.
3. **Acceptance criteria**: at least one per requirement, each referencing its
   requirement number and written so a test or command can prove it.
4. **Non-goals**: things someone might expect that this feature will not do.
5. **Assumptions**: decisions you made without asking me.
6. **Open risks**: anything that could make the build harder than it looks.

Then show me the assumptions and ask for approval. If I change anything, update the
file and ask again. The session is finished when I approve.
