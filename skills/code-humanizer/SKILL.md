---
name: code-humanizer
description: |
  Rewrite LLM-looking code into human-looking code while preserving behavior.
  Focus on practical control flow, explicit data movement, and readable logic.
user-invocable: true
---

# Code-Humanizer

Turn code that reads like generated output into code that reads like it was written by an experienced engineer.

## Use this skill when
- Code is technically correct but over-abstracted.
- Function names are generic and disconnected from the domain.
- Data flows are hard to follow.
- There are too many tiny helpers with little value.
- Logic is split in a way that hides the real workflow.

## Hard rules
1. Keep behavior identical unless a bug fix is explicitly requested.
2. Prefer direct control flow over indirection.
3. Name things by intent and domain, not by implementation trivia.
4. Keep parsing and transformation steps visible in order.
5. Keep helper functions only when they reduce mental load.
6. Avoid framework-like architecture in short scripts.
7. Keep comments sparse and factual.
8. Fail clearly with concrete error messages.

## Human code signals to optimize for
- One readable path from input to output.
- Locals that tell a story (`stage`, `parts`, `flag`) instead of placeholders.
- Pragmatic error handling (`continue` on malformed fragments, hard fail on missing core payload).
- Simple loops and conditionals over nested functional tricks.
- Output that is useful for operators, not generic debug spam.

## LLM patterns to remove
- Boilerplate wrappers around one-line operations.
- Premature abstraction.
- "Utility" layers with no reuse.
- Decorative type usage that does not improve readability.
- Exhaustive but low-signal comments.

## Refactor workflow
1. Identify input boundary, core transform, output boundary.
2. Inline helpers that hide simple logic.
3. Keep only helpers tied to distinct concepts.
4. Rename variables/functions to domain terms.
5. Re-run and diff observable behavior.
6. Trim wording and prints to operator-relevant output.

## Mini examples

1. 
Before:
```python
def process_data(blob):
    return helper_a(helper_b(helper_c(blob)))
```

After:
```python
def parse_payload(html):
    stage = extract_stage(html)
    parts = resolve_format_calls(stage)
    return build_result(parts)
```
2. 
Before:
```python
def process_user_data(user_data):
    # Check if user data exists before processing
    if user_data is not None:
        # Iterate through each user in the provided data
        for user in user_data:
            # Process the user if they are active
            if user.get("active") == True:
                process_user(user)
```

After:
```python
def process_users(users):
    for user in users or []:
        if user.get("active"):
            process_user(user)
```


The second version is not shorter by accident. It is easier to reason about.
