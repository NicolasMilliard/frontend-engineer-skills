---
name: frontend-codebase-discovery
description: Build an evidence-backed mental model and ordered reading path for an unfamiliar frontend codebase, with particular strength in JavaScript and TypeScript repositories. Use when a developer or coding agent asks to explore, understand, map, or onboard before contributing, including as the first phase of a larger task. Do not use when repository inspection is merely incidental to change review, specific-bug diagnosis, feature planning or implementation, broad quality audits, or exhaustive repository documentation.
---

# Frontend Codebase Discovery

Orient a newcomer around the codebase by explaining what matters first, how the
frontend works at the requested depth, and where to continue reading. Optimize
for a useful mental model rather than repository coverage.

## Scope

Investigate an existing frontend application or the frontend surface of a
larger repository. Center the user-named application, package, domain, or
contribution area when one is provided.

Keep the discovery phase read-only. Do not install dependencies, start services,
or create persistent onboarding documentation unless the user asks. When
discovery precedes an authorized implementation, finish the mental model before
editing. Do not turn the report into code review, bug diagnosis, architectural
redesign, or a technical-debt inventory.

## Select the mode

Interpret depth and diagram preferences from ordinary language. Also recognize
`depth=low|medium|high` and `diagram=true|false` as prompt conventions, not as
portable CLI flags or slash-command arguments.

- Use `low` for a quick orientation: “what is this repository and where are the
  important things?”
- Use `medium` for engineering understanding: “how does this frontend work?”
- Use `high` for contribution-ready understanding: “what must I understand
  before modifying this safely?”
- Generate a diagram only when the user requests one and a visual relationship
  would add value.

Default to `medium` and `diagram=false` when the request gives no contrary
signal. State the effective mode in the report. Ask about depth only when the
choice would materially change cost or scope.

## Discovery depth

The levels are cumulative in understanding, not in file count.

### Low

Identify the apparent product purpose, available documentation, repository and
package topology, primary applications, framework and major dependencies,
feature or domain map, routing approach, high-level state and data strategy, and
the first files worth reading. Stop when the main landmarks and responsibilities
are clear. Treat this as a fast index into the repository, not a compressed
architecture report. Once those landmarks are identifiable, do not follow
routes through individual components, hooks, API calls, or tests; their
implementation behavior belongs at deeper levels.

### Medium

Build on the orientation by explaining application and feature boundaries,
important components, hooks, services, state ownership, API and data-fetching
patterns, shared abstractions, and runtime boundaries. Cover authentication,
forms, validation, errors, testing, styling, design systems, and build tooling
only when they materially shape this repository. Trace a representative path
far enough to show how the parts collaborate. Summarize that path at
responsibility and boundary level; do not narrate every function, validation
stage, error variant, dependency, or test module.

### High

Build on the engineering model by tracing the user-named or most architecturally
important flows across UI, application logic, APIs, state or cache, and back to
the UI. Explain side effects, synchronization, loading and failure paths,
cross-feature dependencies, central abstractions, non-obvious constraints, and
evidenced complexity or performance-sensitive boundaries relevant to safe
modification. Stop when the safe-change implications are understandable; do not
attempt exhaustive coverage.

## Discovery workflow

1. Establish the repository root, effective mode, target surface, and any
   sampling needed.
2. Read applicable repository instructions and high-signal documentation such
   as the README, contribution guide, `/docs`, and ADRs.
3. Inspect package and workspace manifests, framework and build configuration,
   and the repository tree to map applications and package responsibilities.
4. Locate application entry points, routing, feature or domain boundaries, and
   shared infrastructure.
5. Follow implementation paths only as far as the selected depth requires.
6. Cross-check important documentation claims against current configuration or
   code, preserve conflicting evidence, and stop when the depth question is
   answered.

Prefer files that reveal ownership, a runtime boundary, a source of truth, or a
high-use abstraction. Skip generated output, vendored code, snapshots, barrel
files, and lockfile contents unless they resolve a specific question. A declared
dependency proves presence, not architectural importance.

For a monorepo, identify the primary applications and summarize package groups
at low depth; do not enumerate every package, and disclose any sampling. For
deeper work, prefer the user-named target, then an application identified as
primary by root documentation and default development or build scripts. If
those signals leave multiple materially different candidates, ask the user;
without clarification, remain at map-level depth and disclose the limit. Do not
imply equal analysis of every package.

## Evidence discipline

Distinguish material claims consistently:

- State facts directly verified in the repository plainly, with concrete
  repository-relative paths or symbols. Do not prefix them with a status
  label.
- **[Inferred]** for a reasoned interpretation. Cite its verified basis and use
  appropriately tentative language.
- **[Unknown]** for missing, inaccessible, stale, or conflicting evidence. State
  what would resolve it.

Treat documentation as evidence of intended behavior until current code
corroborates it. Do not convert absence into a fact, infer product behavior from
names alone, or present a suspicious pattern as a defect merely because it was
encountered during discovery.

## Output

Use the output contract for the selected depth. Do not use the detailed
medium/high structure for `low`.

### Low output

Draft for 160–180 words, then count and trim. The complete rendered report must
contain no more than 200 visible words; headings, evidence labels, and
reading-path explanations count. Only a Mermaid block the user explicitly
requested is excluded. If a complex repository cannot fit, disclose the chosen
surface under `Discovery scope` and defer the rest rather than exceeding the
limit.

Use only these sections, appending `Diagram` only when one is included:

```markdown
## Discovery scope

<one compact bullet covering depth, diagram status, inspected surface, and limits>

## Orientation

<at most four one-sentence, path-supported bullets covering only the product,
runtime/repository shape, main feature areas, stack/data approach, and one
material caveat when present; prefix only inferred claims>

## Start here

1. `relative/path` — <why to read it and the mental model it provides>

## Unknowns

<at most two material unknowns, or “None material within the inspected scope.”>

## Diagram

<only when requested and useful>
```

Include at most three `Start here` entries. Each entry names one path; do not
bundle extra files beneath it. Omit feature implementation details, individual
UI states, test-case behavior, cache mechanics, request handling, and a separate
changes/read-only recap.

### Medium output

Draft for 350–425 words, then count and trim. The complete rendered report must
contain no more than 500 visible words; only a Mermaid block the user explicitly
requested is excluded. For a larger repository, select and disclose the primary
surface rather than expanding the report.

Use these sections in order. Include `Engineering conventions` only when they
materially affect how engineers work in this repository, and append `Diagram`
only when one is included.

```markdown
## Discovery scope

<one compact bullet covering depth, diagram status, inspected surface, and limits>

## Working model

<at most four path-supported bullets covering product, runtime/repository
shape, main feature boundaries, stack, and state/data ownership; prefix only
inferred claims>

## Architecture and data flow

<one representative flow in at most five boundary-level bullets or steps>

## Engineering conventions

<at most three material conventions; omit the section when none are evidenced>

## Start here

1. `relative/path` — <why to read it and the mental model it provides>

## Unknowns

<at most three material unknowns, or “None material within the inspected scope.”>

## Diagram

<only when requested and useful>
```

Include at most five `Start here` entries, with one path and one sentence per
entry. Do not add separate product, feature, repository, or technology
inventories; synthesize their important parts into `Working model`.

### High output

Use the following structure:

```markdown
## Discovery scope

- **Depth:** high
- **Diagram:** not requested | included | omitted as unhelpful
- **Inspected:** <repository surface>
- **Limits:** <sampling, inaccessible evidence, or “None material”>

## Working model

<compact path-supported orientation; prefix only inferred claims>

## Product and feature map

<product purpose, applications, domains, and feature responsibilities>

## Repository and tech map

<topology, stack, entry points, routing, and major infrastructure>

## Architecture and data flow

<ownership, runtime boundaries, and contribution-relevant flows>

## Engineering conventions

<only conventions that materially affect contribution>

## Contribution constraints

<safe-change implications, central dependencies, and evidenced hotspots>

## Start here

1. `relative/path`
   - **Why now:** <why this belongs at this point>
   - **Learn:** <mental model it provides>

## Unknowns

- **[Unknown]** <question and the evidence needed to resolve it>
```

High explains causal paths and safe-change implications; it does not merely add
more inventory to medium. Include at most 15 reading-path entries. This is a
ceiling, not a quota: order them by mental-model dependency and never pad the
list. If no material unknown remains within the inspected scope, say so rather
than inventing one.

When a diagram is requested, read
[Mermaid diagram guidance](references/mermaid.md) before producing it. If none
would improve the report, mark it `omitted as unhelpful` and explain the reason
briefly in `Discovery scope`. Append a `Diagram` section only when a diagram is
included.
