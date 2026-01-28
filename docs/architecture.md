# Architecture

`tooldocs` adds a documentation layer on top of `toolmodel`.
It does not change schemas; it augments them with guidance and examples.

## Tiered disclosure

```mermaid
flowchart LR
  A[DescribeTool] --> B[summary]
  A --> C[schema]
  A --> D[full]
  D --> E[examples]
```

## Resolution sequence

```mermaid
sequenceDiagram
  participant Client
  participant Docs as tooldocs
  participant Index as toolindex

  Client->>Docs: DescribeTool(id, schema)
  Docs->>Index: GetTool(id)
  Index-->>Docs: tool
  Docs-->>Client: ToolDoc (schema)
```

## Progressive disclosure contract

- `DetailSummary`: short text only
- `DetailSchema`: full tool + derived schema info
- `DetailFull`: schema + notes + examples

## Resolution modes

- via `toolindex.Index` (preferred)
- via a custom `ToolResolver` function
