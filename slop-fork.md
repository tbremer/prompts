# Write a technology-agnostic product spec for this repository

Study the repository in the current working directory and write a product requirements document that describes everything the product does, in terms of behavior rather than implementation.

## Who reads this and why

The document will be handed to a group of AI agents who will rebuild this product from scratch, working in parallel. They may use a different language, framework, storage layer, or infrastructure. Assume they will not have access to this repository and cannot ask you questions, so the document is everything they know about the product. Anything missing from it will be missing from the rebuild, and anything vague will be guessed at differently by different agents.

That gives you two goals that pull against each other, and managing the tension is the main work of this task:

- **Be complete and precise about behavior.** Two teams working only from your document should produce products a user could not tell apart.
- **Stay silent about implementation.** Nothing in the document should push the rebuild toward the current language, libraries, architecture, or code structure.

## Separating behavior from implementation

The test is whether a user or an external system would notice if it changed. If they would, it is behavior and belongs in the document with full precision. If they would not, leave it out.

Some cases that are easy to get wrong:

- **Implementation that encodes a rule.** "Sessions are stored in Redis with a 30 minute TTL" becomes "A signed-in session ends after 30 minutes of inactivity." Keep the rule and drop the mechanism.
- **External contracts.** Record these exactly, because users, scripts, and other systems depend on them. They include command names and flags, HTTP routes and payload shapes, file formats the product reads or writes, configuration keys, environment variable names, exit codes, wire protocols, and user-visible messages.
- **Technology that is the product.** If the repository is an editor extension, a plugin for a specific platform, or a client for a specific protocol, that technology is part of what the product is. Keep it and state it as a constraint, with the reason.
- **Third-party services.** State the capability as the requirement ("accept card payments and issue refunds"). Name the current provider once, in the integrations section, since existing accounts or data may be tied to it.
- **Algorithms.** When results depend on the algorithm (ranking, scoring, pricing, rounding, sort order, identifier formats), describe the rules precisely enough to reproduce the same outputs. When the algorithm only affects speed, omit it.
- **Names.** Do not use identifiers from the source (functions, classes, files, tables, modules) unless they are externally visible. Use the product's domain vocabulary and define it in the glossary.

## How to investigate

Start with breadth. Read the README, the documentation, the manifests, the example configuration, and the directory layout to learn what the product is and who uses it.

Then list every entry point, meaning every way behavior gets triggered:

- commands
- routes
- screens
- public library exports
- scheduled jobs
- queue consumers
- webhooks
- extension hooks
- startup and shutdown behavior

This list is your coverage checklist. By the end, every entry point must be accounted for by at least one feature.

Then go deep. Trace each entry point through the code and record:

- inputs and their validation rules
- defaults
- what happens on success
- what happens on each kind of failure
- permission checks
- side effects, such as records created, notifications sent, or external calls made
- limits
- ordering guarantees
- behavior under concurrent use
- state transitions

Other sources are worth reading closely:

- **Tests:** usually the best record of intended behavior and edge cases.
- **Schemas and migrations:** these reveal the data model and its invariants.
- **Configuration code:** this reveals options and defaults.
- **Message catalogs:** these reveal the user-visible text.

If the repository is large and you can delegate to subagents, split the work by product area. Give each subagent the rules in this prompt and the feature format below, then merge their findings and reconcile the vocabulary yourself.

You may run read-only or self-contained commands, such as help output or the test suite, to confirm behavior. Do not run anything that reaches external services or has effects outside the repository. Do not modify anything in the repository other than writing the output document.

## Staying grounded

Document only what you have verified in the code:

- **Code and docs disagree:** the code is what the product does. Describe that and note the discrepancy.
- **Something you could not determine:** put it in the open questions section instead of guessing.
- **Behavior that looks unintended:** describe what the product actually does and flag it in that feature's notes, so the rebuild team can decide whether to preserve it. Do not silently fix it.
- **Improvements:** do not add any.
- **Dead code, unfinished work, and features disabled behind flags:** list these in their own section, not among the features.

## The document

Write the result to `PRD.md` at the repository root. If it would run past roughly 1,500 lines, write a `prd/` directory instead, with `README.md` holding the shared sections and one file per feature area.

Use these sections:

1. **Overview.** What the product is, who uses it, and what problems it solves, in a few paragraphs.
2. **Users and roles.** Every human and system actor, and what each is permitted to do.
3. **Glossary and domain model.** The product's terms, its entities and their attributes, relationships, invariants, and lifecycle states. Keep this conceptual and do not turn it into a storage schema. Every feature must use these terms exactly as defined here.
4. **External interfaces.** The exact contracts described above, so that features can refer to them.
5. **Features.** Grouped by product area, in the format below.
6. **Cross-cutting behavior.** Rules that apply across features, such as authentication, authorization, error conventions, pagination, localization, time handling, and auditing.
7. **Quality requirements.** Performance, scale, security, reliability, and compatibility requirements that the repository gives evidence for, stated measurably. Do not invent targets.
8. **Integrations.** For each external system: the capability needed, the current provider, and what is exchanged.
9. **Platform constraints.** Any technology that is part of the product's identity, and any compatibility obligations to existing data or clients.
10. **Suggested build order.** Which features are foundations that must exist first, and which groups can then be built in parallel.
11. **Incomplete or disabled functionality.**
12. **Open questions.**
13. **Coverage table.** Each entry point mapped to the feature IDs that cover it.

### Feature format

```
### F-012: Short descriptive name

**Summary:** One or two sentences on what this lets an actor do and why.
**Actors:** Who can use it.
**Depends on:** F-003, F-007

**Behavior:** The full flow in prose or numbered steps, including rules,
defaults, limits, and side effects.

**Failures and edge cases:** Each condition and exactly what happens.

**Acceptance criteria:** Checkable statements about observable outcomes.

**Notes:** Suspected bugs, uncertainties, discrepancies with documentation.
```

Size each feature so that one agent could build and verify it in a single working session. An agent may be handed one feature plus the shared sections and nothing else, so each feature must be understandable on those terms. Refer to other features by ID instead of repeating them, and keep IDs stable once assigned.

Write acceptance criteria as outcomes observable through the product's external interfaces, with concrete values. "Submitting a name longer than 64 characters is rejected with the message 'Name is too long' and nothing is saved" is usable. "Input is validated" is not.

## Before you finish

Walk the entry-point list and confirm that each one maps to a feature. Then reread the document as someone who has never seen this repository and is building in a different language. Look for:

- **Leaked implementation details:** library names, file paths, and class names.
- **Vague phrases:** wording such as "handles errors appropriately" that should be replaced with the actual rule.
- **Inconsistent terms:** any term used differently from its glossary definition.

In your final reply, summarize what you wrote, give the number of features per area, and name the areas where your coverage is thin or where you could not verify behavior.
