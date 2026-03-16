# Scenario 1: Naive Prompt

## Goal

To show that without a standard the model response is unstructured, incomplete, and not directly comparable with responses from other models.

## Prompt

Design a user registration system. Requirements:
- registration by email
- storing user data
- sending a confirmation email
- action logging

Return: a list of functions, a list of tables, the relations between them, and function contracts.

## What we do

We send this prompt to Claude, GPT-4, and Gemini. We save the responses without editing them.

## What we expect to see

- Different response formats (text, lists, tables, pseudocode)
- Different levels of detail (one model gives 2 functions, another 5)
- Different names for the same entities
- Missing error path
- No possibility of automatic comparison

## What we measure

Nothing—and that is precisely the problem. The responses are incomparable in form, so measurement requires manual interpretation.

## Takeaway for the paper

Without a fixed structure and form, model responses are text to be read rather than data to be analyzed.
