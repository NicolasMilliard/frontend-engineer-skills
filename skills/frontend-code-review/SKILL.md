---
name: frontend-code-review
description: Review bounded React, Next.js, and browser-facing TypeScript code changes as an experienced PR reviewer. Use when the user explicitly asks for review, critique, PR feedback, or merge-readiness of a diff, component, hook, route, or frontend module. Do not use for feature implementation, diagnosis of a known bug, architecture planning without code, style-only linting, backend-only TypeScript, or exhaustive security, performance, or accessibility audits.
---

# Frontend Code Review

Review existing frontend changes for a few defensible, useful findings. Optimize
for merge decisions, not comment volume.

## Scope

Review a diff, PR, snippet, component, hook, route, or small frontend module and
the nearby context needed to understand it. Do not imply exhaustive coverage of
an entire repository. Review the full supplied change when the user names a PR
or diff; if only a subset is available or practical, identify the exact subset
and omissions and do not claim whole-change merge readiness. For an unbounded
repository request, select and disclose a sensible review surface or ask the
user to bound it when that choice would materially affect the result.

Review rather than rewrite. Do not edit code unless the user separately asks for
implementation.

## Establish context first

Before forming findings:

- Read the request, PR description, or issue to understand intended behavior.
- Inspect the changed code and relevant call sites, types, tests, configuration,
  and local conventions.
- Trace inputs, outputs, state ownership, async transitions, user interaction,
  failure paths, and server/client boundaries that the change touches.
- State an assumption only when it affects a conclusion. Do not invent product
  requirements or treat missing context as proof of a defect.

Focus on issues introduced or exposed by the reviewed change. Mention a
pre-existing issue only when the change materially worsens it or it directly
threatens the reviewed behavior.

## Load relevant guidance

- Read [state and data flow](references/state-and-data-flow.md) when the change
  involves React state, effects, async work, remote data, caching, URL state, or
  a Next.js server/client boundary.
- Read [component contracts](references/component-contracts.md) when the change
  involves component or hook APIs, TypeScript contracts, forms, semantics,
  keyboard/focus behavior, or test boundaries.

Do not load a reference merely to complete a checklist.

## Review workflow

1. Check correctness first: runtime behavior, invariants, data exposure, stale
   values, races, failure paths, and essential interaction.
2. Review responsibilities and boundaries: ownership of state and remote data,
   domain boundaries, server/client placement, and data flow.
3. Review component and type contracts: invalid states, API clarity, domain
   invariants, and whether responsibilities remain understandable.
4. Consider maintainability, performance, accessibility, UX states, and testing
   only where the change creates a concrete risk.
5. Qualify, deduplicate, and prioritize candidate findings before writing them.

Report a candidate only when it has all of the following:

- the smallest relevant location or set of symbols;
- a plausible trigger or scenario;
- a concrete user, system, or maintenance impact;
- evidence from the code or an explicitly stated assumption; and
- a proportionate, actionable direction.

If a candidate lacks one of these, investigate further, ask a genuine question,
or omit it. For maintainability findings, concrete caller burden or evidenced
change pressure can supply the scenario and impact; a runtime failure is not
required. Merge duplicate symptoms under their root cause.

## Judgment

- Prefer simple code when another abstraction would not remove a real source of
  complexity or change.
- Prefer deriving values over storing synchronized copies. Treat effects as a
  tool for synchronization with external systems, not default data-flow glue.
- Distinguish local interaction state, URL state, shared client state, and server
  state instead of moving all state toward one preferred library.
- Keep responsibilities and domain boundaries explicit, but suggest refactors
  only when they materially improve correctness, clarity, or changeability.
- Treat accessibility failures as correctness problems and calibrate their
  severity from the affected interaction.
- Consider loading, error, empty, partial, and refetch states when the relevant
  transition exists.
- Raise a performance issue only with a plausible cost, scale, or hot path. Do
  not prescribe memoization from syntax alone.
- Tie testing recommendations to a named behavior or regression risk. Do not ask
  for tests generically.
- Explain trade-offs and respect established project conventions. Do not rewrite
  working code to enforce personal taste.

The review lenses are not quotas. It is valid for a category, or the entire
review, to have no findings.

## Severity

- **Blocking:** A supported path to material incorrect behavior, crashes, data
  loss or exposure, violation of an externally relied-on contract, or a broken
  essential interaction. Resolve before merge.
- **Recommended:** A material maintainability, boundary, state, accessibility,
  UX, performance, type-safety, or nonessential regression risk that does not
  make the change demonstrably unsafe to merge.
- **Optional:** A local, low-risk simplification with clear net value. Exclude
  formatting, taste, and speculative abstraction.

Uncertainty alone is not a severity. If missing context determines whether a
problem exists, put a concise question or assumption in the review instead of
presenting speculation as a finding.

## Output

Use this structure:

```markdown
## Review summary

- **Intent:** <one-sentence description of the intended change>
- **Assessment:** <merge-relevant conclusion limited to the reviewed scope>

## Blocking issues

- **<Concrete failure, not a category label>** — `path:line`
  <Trigger, impact, evidence, and smallest credible correction.>
```

Always include `Blocking issues`; write `None found in the reviewed scope.` when
empty. Include `Recommended improvements`, `Optional improvements`, and `Open
questions / assumptions` only when they contain useful content.

Order findings by severity and then impact. Use a symbol or component name when
trustworthy line numbers are unavailable. Do not add numeric confidence scores,
generic praise, a checklist recap, or code patches unless requested.
