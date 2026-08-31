# Mermaid diagrams

Use Mermaid as the default diagram format because the result remains text-based,
versionable, diffable, and directly reusable in Markdown. Never generate a
raster image for this skill.

## Choose the diagram

Generate a diagram only when it makes relationships easier to understand than a
short list.

When the user requests a diagram without naming its subject, use one stable
system-architecture view at every depth:

- When no baseline exists, choose applicable anchors in this order: delivery or
  deployment runtime, application runtimes, global composition or state layer,
  routing, feature groups, and external systems or sources of truth.
- Place delivery and runtime entry points on the left, application composition
  and routing in the center, and external systems or sources of truth on the
  right. Use `flowchart LR`.
- At `low`, show only the main runtime, application, feature-group, and external
  system landmarks.
- At `medium`, retain those same anchors and expand one or two of them to show
  important feature, state, or data boundaries. Do not replace the overview
  with a representative feature flow.
- At `high`, retain the architecture view and add a second focused flow only
  when it materially helps explain the contribution area.

Use a focused runtime, data, or user-flow diagram as the sole diagram only when
the user explicitly names that subject or scopes discovery to that flow. Use
`flowchart TB` only for a specifically requested repository or module hierarchy.

If a lower-depth diagram is available in the conversation or supplied by the
user, treat it as the baseline. Preserve its direction, node identifiers,
labels, and supported edges; refine it by adding or decomposing nodes, changing
existing elements only when better evidence requires a correction.

Prefer one diagram. At `high` depth, use two only when the architecture and
focused flow are distinct and both materially aid onboarding.

Do not create a node per file, mirror the directory tree, or diagram unknown
relationships. A diagram is a selected architectural view, not an inventory.

## Conventions

- Use lowercase `snake_case` node identifiers and concise quoted labels.
- Group by application, package, feature, or runtime boundary with at most one
  level of `subgraph` nesting.
- Draw observed relationships with solid arrows (`-->`) and inferred
  relationships with dashed arrows (`-.->`). Explain the inference in prose.
- Keep one direction throughout a diagram.
- Use 6–10 nodes at `low` and at most 12 nodes at `medium` or `high`. Limit each
  diagram to 18 edges. Split by concern rather than shrinking labels or adding
  nesting.
- Cite supporting repository paths in the surrounding report, not inside node
  identifiers.
- Omit custom themes, initialization blocks, click handlers, decorative styles,
  and generated images.

Return each diagram in a fenced `mermaid` block. Check that every node identifier
is declared, every subgraph is closed, labels are quoted when they contain spaces
or punctuation, and edge syntax matches the intended evidence status.
