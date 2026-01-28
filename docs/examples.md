# Examples

## Schema-level detail

```go
doc, _ := store.DescribeTool("github:get_repo", tooldocs.DetailSchema)
fmt.Println(doc.Tool.InputSchema)
```

## Full detail with examples

```go
full, _ := store.DescribeTool("github:get_repo", tooldocs.DetailFull)
for _, ex := range full.Examples {
  fmt.Println(ex.Title, ex.Args)
}
```

## Resolver-only usage (no toolindex)

```go
store := tooldocs.NewInMemoryStore(tooldocs.StoreOptions{
  ToolResolver: func(id string) (*toolmodel.Tool, error) {
    if id != "tickets:create" {
      return nil, fmt.Errorf("unknown: %s", id)
    }
    t := toolmodel.Tool{Namespace: "tickets"}
    t.Name = "create"
    t.Description = "Create a support ticket"
    t.InputSchema = map[string]any{"type": "object"}
    return &t, nil
  },
})
```
