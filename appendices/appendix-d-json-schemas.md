# Appendix D — JSON schemas and validation notes (synchronized clean version)

## D.1. Purpose

JSON representations fix machine-processable forms of the components `G` and `Σ`:

- `graph.schema.json` / `graph.json`
- `tables.schema.json` / `tables.json`

## D.2. graph.schema.json

The schema describes:

- `nodes`
- `edges`
- `contract`
- `refinement_maps`

The current JSON Schema validates the structural form of the document. Additional semantic checks are performed by a separate validator.

## D.3. tables.schema.json

The schema describes:

- the `tables` array
- fields, keys, kinds, and constraints
- the link via `signature_ref` from `graph`

## D.4. Validation result for the source files

The original `graph.json` and `tables.json` pass JSON Schema validation. At the same time, one semantic mismatch was found in `graph.json`: the internal function `fn-create-user` refers in `Tout` to the external id `fn-send-confirmation`, while the internal trigger edge points to `fn-send-confirmation-internal`.

In the clean package, this mismatch is fixed in `graph.json`.

## D.5. Cross-file integrity

For every table node in the graph file, there must exist a `signature_ref` matching `table.id` in the tables file. For every function, the contract must be consistent with the edges. For every `refinement_map`, the internal contracts must be consistent with `internal_edges`.

## D.6. Clean examples

- `graph.json` — aggregated graph example with a consistent refinement map.
- `tables.json` — synchronized tables example.
