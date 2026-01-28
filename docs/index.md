# tooldocs

`tooldocs` provides progressive documentation and examples for tools defined in
`toolmodel` and discovered via `toolindex`.

[![Docs](https://img.shields.io/badge/docs-ai--tools--stack-blue)](https://jonwraymond.github.io/ai-tools-stack/)

## Deep dives
- Design Notes: `design-notes.md`
- User Journey: `user-journey.md`

## Motivation

- **Token efficiency**: fetch details only when needed
- **Better tool usage**: schema + examples reduce call errors
- **Usability**: humans and agents see consistent, structured guidance

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

## Usability notes

- Summary and schema are safe defaults for discovery
- Examples are capped to prevent token blowups
- Notes and external refs are optional but highly recommended

## Next

- Detail tiers and resolution: `architecture.md`
- Registration and caps: `usage.md`
- Examples: `examples.md`
- Design Notes: `design-notes.md`
- User Journey: `user-journey.md`

