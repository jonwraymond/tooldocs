# tooldocs

`tooldocs` provides progressive documentation and examples for tools defined in
`toolmodel` and discovered via `toolindex`.

## What this library provides

- Detail tiers: summary -> schema -> full
- Example payloads with size caps
- Optional tool resolution through `toolindex` or a custom resolver

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

- Detail levels and data flow: `architecture.md`
- Registration and caps: `usage.md`
- Example docs + examples: `examples.md`
