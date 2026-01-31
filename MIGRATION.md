# Migration Guide: tooldocs to tooldiscovery/tooldoc

This guide explains how to migrate from the deprecated `github.com/jonwraymond/tooldocs` package to `github.com/jonwraymond/tooldiscovery/tooldoc`.

## Import Path Changes

Replace all imports from `tooldocs` to `tooldiscovery/tooldoc`:

| Old Import | New Import |
|------------|------------|
| `github.com/jonwraymond/tooldocs` | `github.com/jonwraymond/tooldiscovery/tooldoc` |

## Step-by-Step Migration

### 1. Update go.mod

Remove the old dependency and add the new one:

```bash
# Remove old dependency
go mod edit -droprequire github.com/jonwraymond/tooldocs

# Add new dependency
go get github.com/jonwraymond/tooldiscovery
```

### 2. Update Import Statements

**Before:**

```go
import (
    "github.com/jonwraymond/tooldocs"
)
```

**After:**

```go
import (
    "github.com/jonwraymond/tooldiscovery/tooldoc"
)
```

### 3. Update Package References

Replace all occurrences of `tooldocs.` with `tooldoc.`:

| Old Reference | New Reference |
|---------------|---------------|
| `tooldocs.NewInMemoryStore` | `tooldoc.NewInMemoryStore` |
| `tooldocs.StoreOptions` | `tooldoc.StoreOptions` |
| `tooldocs.DocEntry` | `tooldoc.DocEntry` |
| `tooldocs.DetailSummary` | `tooldoc.DetailSummary` |
| `tooldocs.DetailSchema` | `tooldoc.DetailSchema` |
| `tooldocs.DetailFull` | `tooldoc.DetailFull` |
| `tooldocs.ErrNotFound` | `tooldoc.ErrNotFound` |
| `tooldocs.ErrNoTool` | `tooldoc.ErrNoTool` |
| `tooldocs.ErrInvalidDetail` | `tooldoc.ErrInvalidDetail` |
| `tooldocs.ErrArgsTooLarge` | `tooldoc.ErrArgsTooLarge` |
| `tooldocs.ToolDoc` | `tooldoc.ToolDoc` |
| `tooldocs.ToolExample` | `tooldoc.ToolExample` |

### 4. Example Migration

**Before:**

```go
import (
    "github.com/jonwraymond/tooldocs"
    "github.com/jonwraymond/toolindex"
    "github.com/jonwraymond/toolmodel"
)

store := tooldocs.NewInMemoryStore(tooldocs.StoreOptions{
    Index:       idx,
    MaxExamples: 3,
})

if err := store.RegisterDoc("tickets:create", tooldocs.DocEntry{
    Summary: "Create a new ticket",
    Notes:   "Requires authentication.",
}); err != nil {
    // Handle error
}

doc, err := store.DescribeTool("tickets:create", tooldocs.DetailSchema)
```

**After:**

```go
import (
    "github.com/jonwraymond/tooldiscovery/tooldoc"
    "github.com/jonwraymond/toolindex"
    "github.com/jonwraymond/toolmodel"
)

store := tooldoc.NewInMemoryStore(tooldoc.StoreOptions{
    Index:       idx,
    MaxExamples: 3,
})

if err := store.RegisterDoc("tickets:create", tooldoc.DocEntry{
    Summary: "Create a new ticket",
    Notes:   "Requires authentication.",
}); err != nil {
    // Handle error
}

doc, err := store.DescribeTool("tickets:create", tooldoc.DetailSchema)
```

## API Compatibility

The `tooldiscovery/tooldoc` package maintains API compatibility with the original `tooldocs` package. All types, functions, and error values have the same names and signatures.

## Need Help?

If you encounter issues during migration, please file an issue in the [tooldiscovery repository](https://github.com/jonwraymond/tooldiscovery/issues).
