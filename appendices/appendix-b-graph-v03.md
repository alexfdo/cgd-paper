# Appendix B — Graph specification v0.3 (synchronized clean version)

## B.1. Purpose

The graph **G** is the second component of the triple **T = ⟨Y, G, Σ⟩**. It fixes functions, tables, and typed dependencies between them.

## B.2. Graph definition

**G = (V, E, src, dst, τ_V, τ_E)**, where:

- `τ_V(v) ∈ {function, table}`;
- `τ_E(e) ∈ {consumes, produces, reads, writes, triggers}`.

Duplicate edges with the same `(src, type, dst)` are forbidden.

## B.3. Edge types

- `consumes`: `table → function`
- `produces`: `function → table`
- `reads`: `table → function`
- `writes`: `function → table`
- `triggers`: `function → function`

Links `table → table` are forbidden. Links `function → function` are permitted only for `triggers`. Triggers to `table` are forbidden.

## B.4. Function contract

For every function `f`:

**C(f) = ⟨Cin, Cout, R, W, Tin, Tout⟩**, where each component is derived from graph edges.

## B.5. Subgraph

Let a set of functions `V_func(S)` be chosen.

Then:

- `V_tables(S)` — all tables incident to at least one edge connected to a function from `V_func(S)`;
- `V(S) = V_func(S) ∪ V_tables(S)`;
- `E(S) = { e ∈ E | src(e), dst(e) ∈ V(S) }`.

A table is considered **internal** if all edges incident to it connect it only to functions from `V_func(S)`. The remaining tables form the boundary `∂S`.

## B.6. ComputeSubgraphContract

```text
function ComputeSubgraphContract(S):
    Internal = internal tables of S
    Boundary = V_tables(S) − Internal

    Cin(S)  = boundary tables consumed by functions in V_func(S)
    Cout(S) = boundary tables produced by functions in V_func(S)
    R(S)    = boundary tables read by functions in V_func(S)
    W(S)    = boundary tables written by functions in V_func(S)
    Tin(S)  = external functions that trigger functions in V_func(S)
    Tout(S) = external functions triggered by functions in V_func(S)

    return ⟨Cin(S), Cout(S), R(S), W(S), Tin(S), Tout(S)⟩
```

A boundary table may simultaneously belong to several contract components; this is an admissible and expected case.

## B.7. Trigger internalization and externalization

If a function `g ∈ Tout(f)` is included inside a subgraph `S`, then `g` is removed from `Tout(S)`, and its boundary outputs enter `Cout(S)`. This is **internalization**.

Aggregation performs the inverse operation — **externalization**: `g` returns to `Tout(f_S)`, and the corresponding outputs are removed from `Cout(f_S)`.

## B.8. Decomposition

Replacing a function `f` with a subgraph `S` is admissible if:

1. `S` is induced by `V(S)`;
2. all external links pass through `∂S` and the trigger boundary;
3. the external contract is preserved after trigger internalization is taken into account.

## B.9. Aggregation

A subgraph `S` may be collapsed into an aggregated function `f_S` if the external contract is preserved and there exists a `refinement_map` sufficient to reconstruct the internal structure.

## B.10. Structural checks

The minimum set of structural checks includes:

- type correctness of edges;
- uniqueness of `node.id` and `edge.id`;
- absence of duplicate edges;
- absence of isolated nodes;
- consistency of contracts with the graph;
- presence of PK and kind for every table;
- completeness of mandatory paths.

## B.11. Specification closure

At the graph-level formulation, closure is reached when structural checks are green, there are no unresolved references, contracts are closed, mandatory paths lead to an external effect, and the blocking queue is empty. In the main text, this property is treated as a state of the entire multi-level loop rather than of a single level.
