# tooldocs

`tooldocs` provides progressive documentation and examples for tools defined in
`toolmodel` and discovered via `toolindex`.

## Key APIs

- `Store` interface
- `InMemoryStore` implementation
- `DescribeTool` (summary/schema/full)
- `ListExamples`
- `DocEntry` + `ToolExample`

## Quickstart

```go
store := tooldocs.NewInMemoryStore(tooldocs.StoreOptions{Index: idx})

_ = store.RegisterDoc("github:get_repo", tooldocs.DocEntry{
  Summary: "Fetch repository metadata",
  Notes:   "Requires authentication.",
})

doc, _ := store.DescribeTool("github:get_repo", tooldocs.DetailSchema)
examples, _ := store.ListExamples("github:get_repo", 2)
```

## Next

- Detail tiers and resolution: `architecture.md`
- Registration and caps: `usage.md`
- Examples: `examples.md`
