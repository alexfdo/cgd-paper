# Appendix C — Tables specification v0.2 (synchronized clean version)

## C.1. Purpose

A table signature **Σ(t)** is the third component of the triple **T = ⟨Y, G, Σ⟩**. It defines fields, keys, kind, and constraints.

## C.2. Signature definition

**Σ(t) = ⟨Fields, Keys, Kind, Constraints⟩**.

In the current JSON profile, a field has the form `⟨name, type, required⟩`; the attribute `role` is admissible but does not participate in formal compatibility.

## C.3. Types and keys

The serializable profile uses base types `string`, `integer`, `boolean`, `float`, `timestamp`, `date`, `uuid`, `text`, and `json`. Enum is represented as a constraint over the base string type. Foreign keys are declared explicitly and must refer to an existing table and an existing field.

## C.4. Canonical kinds

For the current version, the following kinds are fixed:

- `entity`
- `event`
- `reference`
- `log`
- `projection`
- `error`

In the current serializable version, every table, including `projection`, must have a canonical PK or a surrogate key.

## C.5. Canonicalization

After canonicalization, the following must hold:

1. presence of a PK;
2. presence of a kind;
3. explicit typing of all fields;
4. explicit FKs wherever referential semantics exists;
5. absence of duplicate field names;
6. compatibility of constraints with field types.

## C.6. Signature compatibility

It is convenient to distinguish:

- **strict equality** `Σ(A) = Σ(B)`;
- **inclusion compatibility** in cases where one table may substitute for another after canonicalization.

For the producer→consumer scenario, the enum rule is formulated as follows:

**Allowed(producer) ⊆ Allowed(consumer)**.

This direction prevents the producer from generating an enum value that the consumer cannot interpret.

## C.7. Version limitations

Version v0.2 still lacks:

- per-consumer field contracts;
- hierarchical table decomposition;
- mutability categories;
- schema migrations and schema versioning;
- complex nested types as part of the formal core of the paper.
