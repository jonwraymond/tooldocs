# tooldocs

> **DEPRECATED**: This package has been merged into `tooldiscovery/tooldoc`.
> Please use `github.com/jonwraymond/tooldiscovery/tooldoc` instead.
>
> See [MIGRATION.md](./MIGRATION.md) for migration instructions.

---

## Migration

All functionality from `tooldocs` is now available in the `tooldoc` subpackage of `tooldiscovery`:

```bash
go get github.com/jonwraymond/tooldiscovery
```

```go
import "github.com/jonwraymond/tooldiscovery/tooldoc"
```

For detailed migration steps, see [MIGRATION.md](./MIGRATION.md).

## Why the change?

The `tooldocs` functionality has been consolidated into `tooldiscovery` to:

- Reduce the number of separate repositories to maintain
- Provide a unified discovery and documentation experience
- Simplify dependency management for consumers

## Archived Documentation

The original README content is preserved below for reference.

---

## Original README (archived)

[![Docs](https://img.shields.io/badge/docs-ai--tools--stack-blue)](https://jonwraymond.github.io/ai-tools-stack/)

Progressive, MCP-aligned documentation for tools defined in `toolmodel` and
discovered via `toolindex`.

### What this library does
- Serves tiered documentation: `summary` -> `schema` -> `full`
- Keeps long docs and examples out of context until explicitly requested
- Backs MCP metatools like `describe_tool` and `list_tool_examples`

### Key behaviors and contracts
- Tool IDs should be canonical `toolmodel.Tool.ToolID()` values
  (for example, `github:get_repo`)
- `DetailSummary` works with docs-only registration
- `DetailSchema` and `DetailFull` require a resolved `toolmodel.Tool`
  (via `toolindex` or `ToolResolver`)
- Example `Args` are deep-copied, normalized to MCP-native shapes
  (`map[string]any`, `[]any`), and validated against caps

### MCP mapping

- `describe_tool` -> `DescribeTool(id, level)`
- `list_tool_examples` -> `ListExamples(id, max)`

The returned `ToolDoc` and `ToolExample` types map directly onto MCP-friendly
result shapes.
