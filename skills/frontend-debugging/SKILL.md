---
name: frontend-debugging
description: Diagnose or fix bounded failures in React, Next.js, and browser-facing TypeScript applications through reproduction, evidence, and hypothesis testing. Use when the user asks why a frontend behavior occurs, needs an intermittent issue isolated, or reports a specific bug that must be understood before correction. Do not use for open-ended code review, feature planning, implementation without an existing failure, or broad performance, security, or accessibility audits.
---

# Frontend Debugging

Diagnose one bounded failure by building an evidence chain from the observed
behavior to its cause. Do not start by rewriting the suspicious code.

## Scope

Use this workflow for a specific existing behavior: a regression, incorrect UI
state, failed interaction, rendering problem, request or cache inconsistency,
hydration issue, intermittent failure, or similar frontend bug.

Bound broad reports before investigating them. Identify the affected user flow,
environment, and smallest relevant project surface. Do not turn diagnosis into a
general code review or architecture redesign.

Describe the smallest credible correction after establishing the cause. Modify
code only when the user asks for a fix.

## Establish the problem

Before proposing causes:

- Separate the observed behavior from the expected behavior.
- Record the known trigger, affected environment, frequency, and timing when
  they matter.
- Inspect the relevant code, tests, configuration, recent changes, runtime
  output, and project conventions.
- Distinguish facts supplied by the user, evidence you observed, and hypotheses
  that still need testing.

Ask only for missing information that changes the next diagnostic step. When a
full reproduction is unavailable, define the smallest safe probe that could
separate the leading hypotheses.

## Workflow

1. Reproduce the failure or identify a reliable observable signal for it.
2. Bound the failure by finding the last correct state and the first incorrect
   state in the relevant user, event, render, data, or request flow.
3. Form a small set of plausible hypotheses. Connect each one to evidence it
   predicts; do not produce an unranked brainstorm.
4. Run the cheapest discriminating checks first. Prefer existing tests, logs,
   browser or network evidence, type checks, and focused commands before code
   changes or broad test suites.
5. Update or discard hypotheses as evidence arrives. Do not preserve an early
   theory merely because it matches the suspicious line of code.
6. Name a root cause only when it explains the trigger, the observed behavior,
   and why the expected path failed.
7. Describe the smallest correction at the layer that owns the cause. Avoid
   compensating for the symptom elsewhere.
8. Recommend regression protection that exercises the failure mechanism through
   the narrowest stable observable boundary.

Trace only the boundaries the evidence touches. These may include React render
and effect timing, state ownership, browser events, request ordering, cache
identity, serialization, or a Next.js server/client transition. Do not inspect
every layer by default.

Prefer read-only investigation. Do not add instrumentation or other temporary
code changes unless the user has authorized edits. If evidence depends on an
environment or action you cannot access, state the exact observation needed
instead of guessing.

## Diagnostic standard

- A symptom is not a root cause.
- A code smell is not evidence that it caused this failure.
- Correlation with a recent change narrows the search but does not prove cause.
- A successful fix should explain why the failure disappears, not merely make
  the current reproduction pass.
- An unresolved diagnosis is valid. Preserve uncertainty and identify the next
  discriminating check.

## Output

Use only the sections that help the current investigation:

```markdown
## Diagnosis

- **Status:** Confirmed | Most likely | Unresolved
- **Observed:** <what happens>
- **Expected:** <what should happen>
- **Root cause:** <cause, only when supported>

## Evidence

<observation and what it rules in or out>

## Smallest correction

<the owning layer and minimal change; include implementation only when requested>

## Regression protection

<the behavior and failure mechanism to protect>

## Next check

<the next discriminating observation when unresolved>
```

Do not use numeric confidence scores. Keep hypotheses out of `Diagnosis` unless
their status is explicit, and do not present unrun checks as evidence.
