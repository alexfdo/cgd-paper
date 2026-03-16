# Scenario 2: Structured Response with the Standard

## Goal

To show that the standard makes responses from different models comparable and their differences measurable.

## Prompt

Design a user registration system. Requirements:
- registration by email
- storing user data
- sending a confirmation email
- action logging

Return the response strictly in two sections:
[Insert the execution profile of standard v1.3]

## Run protocol

- The same prompt for all models
- `temperature = 0` (if the provider allows it)
- Save raw output without editing
- Record: model, date, parameters, scenario

## Result-processing steps

### Step 1: Raw response

Save each model response without changes.

### Step 2: Standard compliance check

| Criterion | Claude | GPT-4 | Gemini |
| --- | --- | --- | --- |
| Two sections (Mermaid + JSON) |  |  |  |
| `flowchart LR` |  |  |  |
| Prefixes `tbl_` / `fn_` |  |  |  |
| `x-kind` for every table |  |  |  |
| `x-pk` for every table |  |  |  |
| `contract` for every function |  |  |  |
| `processing` for every function |  |  |  |
| Rules 1–14 (cross-sectional) |  |  |  |
| Rules 15–19 (inside JSON) |  |  |  |

### Step 3: Normalization

Bring names to canonical form according to the normalization rules:
- `rename` (case, number, format)
- `semantic generalization` (specific name → canonical role)

Record the normalization table for each model.

### Step 4: Reference alignment

Comparison is performed at the `canonical top-level model` level.

| Criterion | Canon | Claude | GPT-4 | Gemini |
| --- | --- | --- | --- | --- |
| Functions | 2 |  |  |  |
| Tables | 6 |  |  |  |
| Edges | 9 |  |  |  |
| RegistrationRequest | yes |  |  |  |
| RegistrationEvent | yes |  |  |  |
| RegistrationError | yes |  |  |  |
| AuditLog | yes |  |  |  |
| NotificationEvent | yes |  |  |  |
| Error path | yes |  |  |  |
| Contracts complete | yes |  |  |  |

## Takeaway for the paper

The standard fixes structure and form. `Standard compliance` is checked automatically. `Reference alignment` reveals substantive differences after normalization. These are two different dimensions of response quality.
