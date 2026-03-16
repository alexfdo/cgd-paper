# Appendix C — ComputeSubgraphContract

## C.1. Purpose

This appendix fixes a minimal formalization of the subgraph contract and the `ComputeSubgraphContract` algorithm used for decomposition, aggregation, and local verification of the structural realization.

## C.2. Preconditions

Let the structural realization be given as a typed directed multigraph

`G = (V, E, src, dst, τ_V, τ_E)`,

where:
- `τ_V(v) ∈ {function, table}`;
- `τ_E(e) ∈ {consumes, produces, reads, writes, triggers}`.

Each function `f` has a contract

`C(f) = ⟨Cin, Cout, R, W, Tin, Tout⟩`,

where all components are fully derived from incident edges of the corresponding type.

## C.3. Subgraph contract

For a chosen subgraph `S`, the set of its vertices is denoted by `V(S)` and decomposed into two parts:

`V(S) = V_func(S) ∪ V_table(S)`,

where `V_func(S)` is the set of vertices of type `function` belonging to `S`, and `V_table(S)` is the set of vertices of type `table` belonging to `S`.

`E(S) = { e ∈ E(G) | src(e) ∈ V(S) ∨ dst(e) ∈ V(S) }` is the set of edges of graph `G` for which at least one endpoint belongs to `V(S)`.

For the subgraph `S`, the contract

`C(S) = ⟨Cin(S), Cout(S), R(S), W(S), Tin(S), Tout(S)⟩`

is defined.

It describes only the external boundary of the subgraph: which tables and functions connect `S` to the rest of the graph.

## C.4. `ComputeSubgraphContract` algorithm

```text
function ComputeSubgraphContract(S):
    V_func(S)  = {v ∈ V(S) : τ_V(v) = function}
    V_table(S) = {v ∈ V(S) : τ_V(v) = table}

    internal_tables = {t ∈ V_table(S) :
        ∀e ∈ E where (src(e)=t or dst(e)=t) :
            (src(e) ∈ V(S) and dst(e) ∈ V(S))}

    interface_tables = V_table(S) \ internal_tables

    Cin(S)  = {t ∈ interface_tables : ∃e ∈ E(S), τ_E(e) = consumes,
               src(e) = t, dst(e) ∈ V_func(S)}

    Cout(S) = {t ∈ interface_tables : ∃e ∈ E(S), τ_E(e) = produces,
               src(e) ∈ V_func(S), dst(e) = t}

    R(S)    = {t ∈ interface_tables : ∃e ∈ E(S), τ_E(e) = reads,
               src(e) = t, dst(e) ∈ V_func(S)}

    W(S)    = {t ∈ interface_tables : ∃e ∈ E(S), τ_E(e) = writes,
               src(e) ∈ V_func(S), dst(e) = t}

    Tin(S)  = {f ∉ V(S) : ∃e ∈ E, τ_E(e) = triggers,
               src(e) = f, dst(e) ∈ V_func(S)}

    Tout(S) = {f ∉ V(S) : ∃e ∈ E, τ_E(e) = triggers,
               src(e) ∈ V_func(S), dst(e) = f}

    return ⟨Cin(S), Cout(S), R(S), W(S), Tin(S), Tout(S)⟩
```

## C.5. Note on interface tables

An interface table may belong to several components of the subgraph contract at once. For example, a table may be consumed by one boundary function and produced by another. This is a normal case: the subgraph contract fixes external relations rather than imposing a single role on a table.

## C.6. Usage

### Decomposition

Under decomposition, the node `f` is replaced with a subgraph `S` such that the following holds:

`C(f) = C(S)`.

Intermediate internal tables are allowed as long as they do not reach the boundary of the subgraph.

### Aggregation

Under aggregation, the subgraph `S` is collapsed into a single node `f_S` with the contract

`C(f_S) = C(S)`.

This makes `ComputeSubgraphContract` the minimal formal mechanism that supports contract-preserving transformations between different levels of detail of the same structural realization.
