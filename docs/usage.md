# Usage

## Register docs

```go
store := tooldocs.NewInMemoryStore(tooldocs.StoreOptions{
  Index:       idx,
  MaxExamples: 3,
})

err := store.RegisterDoc("tickets:create", tooldocs.DocEntry{
  Summary: "Create a support ticket",
  Notes:   "Requires authentication. Supports idempotency via request_id.",
  Examples: []tooldocs.ToolExample{
    {
      Title:       "Minimal",
      Description: "Create a low-priority ticket",
      Args:        map[string]any{"title": "Login broken"},
      ResultHint:  "Ticket object with id and status",
    },
  },
})
```

## Describe tools

```go
doc, _ := store.DescribeTool("tickets:create", tooldocs.DetailSchema)
full, _ := store.DescribeTool("tickets:create", tooldocs.DetailFull)
```

## List examples

```go
examples, _ := store.ListExamples("tickets:create", 2)
```

## Size caps

Examples are validated at registration time:

- `MaxArgsDepth = 5`
- `MaxArgsKeys = 50`
- `MaxDescriptionLen = 300`
- `MaxResultHintLen = 200`
