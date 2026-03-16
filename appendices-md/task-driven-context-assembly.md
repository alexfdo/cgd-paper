# Appendix D — Task-Driven Context Assembly

## D.1. Purpose

This appendix fixes a draft formalization of `task-driven context assembly` as a mechanism for assembling context not from the entire dialogue history, but from the affected objects of the structural realization.

## D.2. Goal

The goal of the mechanism is to obtain a compact and relevant context for the next task by using `R = ⟨G, Σ, C⟩` as the single source of structural localization.

## D.3. Input

The input consists of:
- a new task `q`;
- the current structural realization `R = ⟨G, Σ, C⟩`;
- a history of decisions `H`, containing previous clarifications, accepted assumptions, and unresolved questions.

## D.4. Localizing the task in `R`

First, the task `q` is localized in the structural realization. The result of this step is the initial set of affected objects:

`A₀(q) = F₀ ∪ T₀`,

where:
- `F₀` are the functions explicitly mentioned or semantically corresponding to the task;
- `T₀` are the tables explicitly mentioned or directly connected to the functions found.

In practice, this means anchoring the task to the existing graph nodes, their contracts, and their signatures, rather than to the chat history as a whole.

## D.5. Choosing the local neighborhood

After localization, a local structural neighborhood `N(A₀)` is built.

In the minimal version, it includes:
- the affected functions and tables themselves;
- the tables from `Cin`, `Cout`, `R`, `W` for the functions found;
- the functions from `Tin`, `Tout`;
- neighboring nodes at graph distance one.

The idea is simple: not the entire graph enters the context, but only the nearest structural zone in which the change can realistically affect system behavior.

## D.6. Lifting history by structural keys

After `N(A₀)` has been built, the history `H` is lifted not in full, but only by the structural keys from the neighborhood found.

The following are selected:
- past decisions explicitly related to functions and tables from `N(A₀)`;
- previously recorded blocking or advisory remarks concerning the same objects;
- accepted assumptions that have not yet been materialized in the current version of `R`.

Thus, history ceases to be an independent source of truth and becomes an auxiliary layer indexed by the objects of the structural realization.

## D.7. Assembling task-specific context

The resulting task-specific context `CTX(q)` consists of three parts:
- the current state of the affected objects;
- the local structural neighborhood;
- the relevant historical decisions lifted by the same keys.

That is,

`CTX(q) = Current(A₀ ∪ N(A₀)) ∪ History(A₀ ∪ N(A₀))`.

Unrelated graph fragments and unrelated history are not included in the context, even if they are topically similar to the task.

## D.8. Output

The output is a compact structurally anchored context suitable for:
- modifying `R`;
- repeated verification;
- preparing the next model step.

## D.9. Illustrative example

If the task is “add error logging for registration,” the mechanism localizes it in the registration function, its error path, and the nearest logging sink. The history is then lifted by those same objects: it may turn out that a prior decision already existed to log errors separately from the success path, but that decision was never introduced into the current realization. The task-specific context therefore contains precisely this local zone rather than the full conversation and rather than the full system.

## D.10. Status

In the current paper, `task-driven context assembly` is introduced as an architectural principle and a draft formalization. The full algorithm for neighborhood selection, ranking of historical fragments, and evaluation of context sufficiency remains future work.
