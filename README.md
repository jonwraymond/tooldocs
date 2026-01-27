# tooldocs

Progressive, MCP-aligned documentation for tools defined in `toolmodel` and
discovered via `toolindex`.

## What this library does
- Serves tiered documentation: `summary` -> `schema` -> `full`
- Keeps long docs and examples out of context until explicitly requested
- Backs MCP metatools like `describe_tool` and `list_tool_examples`

## Key behaviors and contracts
- Tool IDs should be canonical `toolmodel.Tool.ToolID()` values
  (for example, `github:get_repo`)
- `DetailSummary` works with docs-only registration
- `DetailSchema` and `DetailFull` require a resolved `toolmodel.Tool`
  (via `toolindex` or `ToolResolver`)
- Example `Args` are deep-copied, normalized to MCP-native shapes
  (`map[string]any`, `[]any`), and validated against caps

## Install

```bash
go get github.com/jonwraymond/tooldocs
```

## Usage (with toolindex)

```go
import (
  "fmt"

  "github.com/jonwraymond/tooldocs"
  "github.com/jonwraymond/toolindex"
  "github.com/jonwraymond/toolmodel"
)

idx := toolindex.NewInMemoryIndex()

tool := toolmodel.Tool{Namespace: "tickets"}
tool.Name = "create"
tool.Description = "Create a ticket"
tool.InputSchema = map[string]any{"type": "object"}

backend := toolmodel.ToolBackend{
  Kind:  toolmodel.BackendKindLocal,
  Local: &toolmodel.LocalBackend{Name: "handler"},
}
_ = idx.RegisterTool(tool, backend)

store := tooldocs.NewInMemoryStore(tooldocs.StoreOptions{
  Index:       idx,
  MaxExamples: 3,
})

if err := store.RegisterDoc("tickets:create", tooldocs.DocEntry{
  Summary: "Create a new ticket",
  Notes:   "Requires authentication.",
}); err != nil {
  // Handle registration errors (for example, args caps)
}

doc, err := store.DescribeTool("tickets:create", tooldocs.DetailSchema)
examples, err := store.ListExamples("tickets:create", 2)
```

## Usage (with ToolResolver injection)

Use this when you do not want a hard dependency on `toolindex`.

```go
import (
  "fmt"

  "github.com/jonwraymond/tooldocs"
  "github.com/jonwraymond/toolmodel"
)

store := tooldocs.NewInMemoryStore(tooldocs.StoreOptions{
  ToolResolver: func(id string) (*toolmodel.Tool, error) {
    if id != "tickets:create" {
      return nil, fmt.Errorf("unknown tool: %s", id)
    }
    t := toolmodel.Tool{Namespace: "tickets"}
    t.Name = "create"
    t.Description = "Create a ticket"
    t.InputSchema = map[string]any{"type": "object"}
    return &t, nil
  },
})
```

Resolver errors are propagated (not masked as `ErrNotFound`) when they prevent
tool resolution.

## Args caps (token safety)

Examples are validated at registration time:
- `MaxArgsDepth = 5` (maximum nesting depth)
- `MaxArgsKeys = 50` (total size: map keys + slice items across all levels)

If caps are exceeded, `RegisterDoc` / `RegisterExamples` return
`ErrArgsTooLarge`.

## Errors

Use `errors.Is` to check:
- `ErrNotFound`: no docs and no tool could be resolved
- `ErrNoTool`: docs exist but no tool is available for `schema` / `full`
- `ErrInvalidDetail`: invalid `DetailLevel`
- `ErrArgsTooLarge`: example args exceed caps

## MCP mapping

- `describe_tool` -> `DescribeTool(id, level)`
- `list_tool_examples` -> `ListExamples(id, max)`

The returned `ToolDoc` and `ToolExample` types map directly onto MCP-friendly
result shapes.

## Concurrency

`InMemoryStore` is safe for concurrent use.

## Version compatibility

See `VERSIONS.md` for the authoritative, auto-generated compatibility matrix.


These versions reflect the aligned baseline across the tool libraries.
