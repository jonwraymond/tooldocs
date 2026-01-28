# Design Notes

This page explains the design choices and error semantics for `tooldocs`.

## Design tradeoffs

- **Progressive detail tiers.** `DetailSummary`, `DetailSchema`, and `DetailFull` keep large docs out of the default path while still allowing rich guidance on demand.
- **Schema-first truth.** `ToolDoc.Tool` (from `toolmodel`) remains the canonical schema source; docs only augment, they do not override.
- **Token safety caps.** Examples, summaries, notes, and args are capped to prevent context bloat in downstream LLM usage.
- **Best-effort schema derivation.** `SchemaInfo` is derived from JSON Schema when possible; missing or complex schema features simply result in empty fields.
- **Mutable-safety.** Args are deep-copied and normalized to MCP-native shapes on registration and retrieval to avoid mutation bugs.
- **Flexible resolution.** Docs can be served from the index or a custom resolver, enabling file-backed or remote sources without changing the interface.

## Error semantics

`tooldocs` exposes sentinel errors for predictable handling:

- `ErrNotFound` – no docs and no tool found for the given ID.
- `ErrInvalidDetail` – unknown detail level requested.
- `ErrNoTool` – schema/full requested but tool is not resolvable.
- `ErrArgsTooLarge` – example args exceed depth/size caps.

### Behavior by level

- **Summary:** works when either docs or tool exist; returns `ErrNotFound` only if both are missing.
- **Schema/Full:** requires `toolmodel.Tool` (from `toolindex` or `ToolResolver`), otherwise returns `ErrNoTool`.

## Extension points

- **Custom resolvers:** provide `ToolResolver` when tools are not in `toolindex`.
- **Alternative stores:** implement `Store` for file-backed or remote doc sources.
- **Docs generation:** generate `DocEntry` from code comments or external sources and register at startup.

## Operational guidance

- Keep examples short and bounded; the caps (`MaxArgsDepth`, `MaxArgsKeys`, etc.) are enforced at registration time.
- Prefer a small number of high-quality examples over many low-signal ones.
- Use `SchemaInfo` only for UI hints and human guidance; use actual schemas for validation.
