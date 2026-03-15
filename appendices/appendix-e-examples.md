# Appendix E — Running examples (synchronized clean version)

## E.1. Example 1 — Birth of the triple

The request produces two functions (`RegisterUser`, `SendConfirmation`) and five tables (`RegistrationRequest`, `User`, `RegistrationEvent`, `AuditLog`, `NotificationEvent`).

Top-level contract:

`C(RegisterUser) = ⟨{RegistrationRequest}, {User, RegistrationEvent}, {User}, {AuditLog}, ∅, {SendConfirmation}⟩`.

Structure: **2 functions / 5 tables / 8 edges**.

## E.2. Example 2 — Decomposition

`RegisterUser` is decomposed into `ValidateInput`, `CheckUniqueness`, `CreateUser`, and `SendConfirmation`; `ValidatedRequest` and `UniqueRequest` are added.

After applying `ComputeSubgraphContract`:

`C(S) = ⟨{RegistrationRequest}, {User, RegistrationEvent, NotificationEvent}, {User}, {AuditLog}, ∅, ∅⟩`.

Closure is not yet reached: the error path is missing.

Structure: **4 functions / 7 tables / 12 edges**.

## E.3. Example 3 — Defects and repair

Defect classification:

1. `ValidateInput` without an error path — **blocking**.
2. `CheckUniqueness` without an error path — **blocking**.
3. No write into `AuditLog` on the error path — **advisory**.
4. `RegistrationEvent` has no internal consumer — **advisory**.

The user chooses `RegistrationError` as the unified error output for the two blocking defects and separately decides to add `AuditLog` on the error path. `RegistrationEvent` is confirmed as an external analytics output.

The top-level contract is refined to `v2`:

`C(RegisterUser) = ⟨{RegistrationRequest}, {User, RegistrationEvent, RegistrationError}, {User}, {AuditLog}, ∅, {SendConfirmation}⟩`.

Closure is reached after this step.

Structure: **4 functions / 8 tables / 16 edges**.

## E.4. Example 4 — Aggregation

The repaired subgraph is collapsed back into `RegisterUser`. The external contract matches `v2`, and the internal structure is preserved through `refinement_map`.

The key formal point is that reversibility of `decompose/aggregate` is **parameterized by the subgraph boundary**. If a trigger is internalized, its boundary outputs enter `Cout(S)`; when externalized, they return to the child function. This rule makes Examples 2 and 4 mutually consistent.

Structure: **2 functions / 6 tables / 9 edges**.

## E.5. Example 5 — Model comparison and normalization

Two LLM models produce different names and different levels of structural completeness. A triple is extracted from each output; discrepancies are then classified and removed not by direct “self-convergence” of the models, but by normalization against the project reference model from Examples 1–4.

The result is a canonical graph and the `v2` contract.

Structure: **2 functions / 6 tables / 9 edges**.

## E.6. Traceability summary

- Example 1: birth of `v1`.
- Example 2: decomposition with trigger internalization, closure not reached.
- Example 3: `v1 → v2`, `2 blocking + 2 advisory`, closure reached.
- Example 4: aggregation, closure preserved.
- Example 5: normalization against the project canonical model, closure preserved.
