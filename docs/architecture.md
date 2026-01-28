# Architecture

`tooldocs` adds a documentation layer on top of `toolmodel`.
It does not change schemas; it augments them with guidance and examples.

```mermaid
flowchart LR
  A[toolindex] --> B[tooldocs]
  B --> C[describe_tool]
  B --> D[list_tool_examples]

  subgraph Tiers
    S[summary]
    H[schema]
    F[full]
  end
  B --> S
  B --> H
  B --> F
```

## Resolution

`tooldocs` can resolve tools in two ways:

- via `toolindex.Index` (preferred)
- via a custom `ToolResolver` function
