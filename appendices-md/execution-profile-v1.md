# Response format

Return the result strictly in two sections. Text outside the sections is not allowed.

## Section 1: Mermaid

Fenced block: ```mermaid

Format: `flowchart LR`

Tables are trapezoids with the `tbl_` prefix:

    tbl_user[/"User (entity)"/]

Functions are rounded rectangles with the `fn_` prefix:

    fn_register_user(["RegisterUser"])

Allowed edge types (must be specified in the edge label):
- `consumes`: `table → function`
- `produces`: `function → table`
- `reads`: `table → function`
- `writes`: `function → table`
- `triggers`: `function → function`

Forbidden: `table → table`, `function → function` without `triggers`.

## Section 2: JSON

Fenced block: ```json

A single object with two keys: `functions` and `tables`.

### functions

The key is the function name (`PascalCase`, verb + noun).  
The value is an object:

```json
{
  "purpose": "one sentence",
  "processing": ["step 1", "step 2", "..."],
  "input": {
    "consumes": { "TableName": "short description" },
    "reads": { "TableName": "short description" }
  },
  "output": {
    "produces": { "TableName": "short description" },
    "writes": { "TableName": "short description" }
  },
  "errors": [
    { "condition": "condition", "result": "result" }
  ],
  "triggers": [
    { "function": "FunctionName", "condition": "condition" }
  ],
  "contract": {
    "Cin": ["tables"],
    "Cout": ["tables"],
    "R": ["tables"],
    "W": ["tables"],
    "Tin": ["functions"],
    "Tout": ["functions"]
  }
}
```

Rules:
- `consumes` / `reads` / `produces` / `writes`: the value must be a short string only, not an object;
- `processing`: each element must describe one action on one line;
- `contract`: arrays are treated as sets; order is irrelevant and duplicates are forbidden;
- empty fields: `{}` for objects, `[]` for arrays.

### tables

The key is the table name (`PascalCase`, singular).  
The value is an object:

```json
{
  "x-kind": "entity | event | reference | log | projection | error",
  "x-pk": "field_name",
  "properties": {
    "field_name": { "type": "type", "format": "format" }
  },
  "required": ["required fields"]
}
```

Field-level extensions (inside `properties`):
- `"x-fk": "Table.field"` — foreign key;
- `"x-unique": true` — unique value.

`x-pk` and `x-fk` are `single-field` only.

Field types:
- `text` → `string`
- `number` → `number` (`integer` is allowed)
- `bool` → `boolean`
- `date` → `string` + `format: date-time`
- `enum` → `string` + `enum: [values]`
- `ref` → target PK type + `x-fk`

Field names and enum values: `snake_case`.

## Section priority

If Mermaid and JSON diverge, JSON is the source of truth.

## Consistency

The sections must be consistent:
- every node in Mermaid = an entry in `functions` or `tables`;
- every edge in Mermaid = an element of `contract`;
- every element of `contract` = an edge in Mermaid;
- `kind` in a Mermaid table label = `x-kind` in `tables`;
- keys in `consumes` / `reads` / `produces` / `writes` = the corresponding arrays in `contract`;
- values in `triggers[*].function` = the `Tout` array in `contract`.
