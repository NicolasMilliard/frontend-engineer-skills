# Component contracts

Use this reference when a change introduces or alters a component, hook, or public prop contract. Review which states and behaviors the interface permits; do not infer quality from component size or abstraction count.

## Responsibility and ownership

**Raise when:** Parent and child both own the same decision, a reusable-looking component silently performs domain-specific work, or hidden side effects make callers unable to predict or coordinate behavior. Prefer the smallest boundary correction that restores one clear owner.

**Do not raise when:** A component is cohesive despite being long, a small component remains clearer inline, or domain knowledge is intentionally encapsulated behind an explicit contract. Do not request extraction solely for file length, reuse speculation, or aesthetic separation.

## Representable states and TypeScript

**Raise when:** Mutually exclusive booleans, loosely related optionals, broad strings, unchecked assertions, or weakened generics let callers construct a state the runtime cannot safely handle. Suggest a discriminated union or narrower type when it makes a real invariant enforceable, and identify the invalid call or failed narrowing that is currently possible.

**Do not raise when:** Flags are genuinely independent, an assertion is justified by a checked boundary or framework invariant, or inference already preserves the needed guarantees. Do not demand elaborate generics, branded types, or exhaustive modeling that adds ceremony without preventing a plausible defect.

## Controlled and uncontrolled APIs

**Raise when:** A component ambiguously mixes `value` and `defaultValue`, changes control mode during its lifetime, copies controlled props into state without a reconciliation rule, or emits events whose meaning does not match the displayed state. Check how resets and prop changes behave, not just the initial render.

**Do not raise when:** A component is consistently controlled or consistently owns self-contained state with a clear initial value. Do not require a controlled API unless a caller actually needs to coordinate, persist, or reset the value.

## Semantics, keyboard, and focus

**Raise when:** An interactive behavior is attached to a non-interactive element, lacks an accessible name or keyboard path, exposes incorrect disabled/selected/expanded semantics, or loses focus at a meaningful transition such as opening or closing a dialog. Prefer the native element whose built-in behavior matches the interaction.

**Do not raise when:** Native semantics already provide the required role and keyboard behavior, or focus is intentionally unchanged and remains usable. Do not add redundant ARIA or recommend recreating button/link behavior with roles and key handlers when a native control fits.

## Observable test boundaries

**Raise when:** The change adds a material behavior or regression path that existing tests do not protect, especially an invalid prop combination, controlled-state transition, keyboard interaction, focus handoff, or async state change. Name the observable setup and outcome the test should cover.

**Do not raise when:** Existing coverage exercises that behavior through a stable boundary, the risk is purely theoretical, or the proposed test would only assert hook calls, internal state, markup snapshots, or another implementation detail. Do not ask for tests merely to increase coverage or mirror every branch.
