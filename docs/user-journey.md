# User Journey

This journey shows how `tooldocs` provides progressive disclosure in an end-to-end agent workflow.

## End-to-end flow (stack view)

![Diagram](assets/diagrams/user-journey.svg)

## Step-by-step

1. **Discovery**: agent finds candidate tools via `search_tools`.
2. **Schema retrieval**: agent requests `detail_level="schema"` to see required parameters.
3. **Full guidance**: agent requests `detail_level="full"` or `list_tool_examples` when extra guidance is needed.
4. **Execution**: agent calls the tool using `toolrun` with validated args.

## Example: register docs and fetch

```go
store := tooldocs.NewInMemoryStore(tooldocs.StoreOptions{Index: idx})

_ = store.RegisterDoc("github:create_issue", tooldocs.DocEntry{
  Summary: "Create a GitHub issue.",
  Notes:   "Use labels sparingly; title is required.",
  Examples: []tooldocs.ToolExample{
    {
      Title:       "Create a bug issue",
      Description: "Create a bug ticket with a label.",
      Args: map[string]any{
        "owner": "acme",
        "repo": "app",
        "title": "Crash on login",
        "labels": []any{"bug"},
      },
      ResultHint: "Returns the issue number and URL.",
    },
  },
})

doc, _ := store.DescribeTool("github:create_issue", tooldocs.DetailFull)
```

## Expected outcomes

- Clear, compact guidance without shipping the full schema by default.
- Predictable errors when docs are missing or a tool cannot be resolved.
- Examples that are safe to include in LLM context.

## Common failure modes

- `ErrNoTool` when requesting schema/full without tool registration.
- `ErrArgsTooLarge` when example payloads exceed caps.
- `ErrInvalidDetail` for invalid detail levels.
