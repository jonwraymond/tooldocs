# API Reference

## Store interface

```go
type Store interface {
  DescribeTool(id string, level DetailLevel) (ToolDoc, error)
  ListExamples(id string, maxExamples int) ([]ToolExample, error)
}
```

## Detail levels

```go
const (
  DetailSummary DetailLevel = "summary"
  DetailSchema  DetailLevel = "schema"
  DetailFull    DetailLevel = "full"
)
```

## ToolDoc

```go
type ToolDoc struct {
  Tool         *toolmodel.Tool
  Summary      string
  SchemaInfo   *SchemaInfo
  Notes        string
  Examples     []ToolExample
  ExternalRefs []string
}
```

## ToolExample

```go
type ToolExample struct {
  ID          string
  Title       string
  Description string
  Args        map[string]any
  ResultHint  string
}
```

## SchemaInfo

```go
type SchemaInfo struct {
  Required []string
  Defaults map[string]any
  Types    map[string][]string
}
```

## StoreOptions

```go
type StoreOptions struct {
  Index        toolindex.Index
  ToolResolver func(id string) (*toolmodel.Tool, error)
  MaxExamples  int
}
```

## InMemoryStore

```go
func NewInMemoryStore(opts StoreOptions) *InMemoryStore
func (s *InMemoryStore) RegisterDoc(id string, entry DocEntry) error
```

## Errors

- `ErrNotFound`
- `ErrInvalidDetail`
- `ErrNoTool`
- `ErrArgsTooLarge`
