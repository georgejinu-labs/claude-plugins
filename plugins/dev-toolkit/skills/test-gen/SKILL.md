---
name: test-gen
description: Generate unit tests for a file or function, matching the project's existing test framework, style, and conventions.
disable-model-invocation: true
---

# Test generator

## Instructions
1. Identify the target: the file/function named in the request, or the currently open/selected file if none is specified.
2. Detect the existing test framework and conventions from sibling/existing test files (naming pattern, assertion library, mocking style, fixture setup). Do not introduce a new framework if one is already in use.
3. Enumerate cases before writing code: happy path, boundary values, error/exception paths, and every branch/conditional in the target code. List them briefly for the user before generating.
4. Write tests that assert behavior (inputs → outputs/side effects), not implementation details — don't assert internal private state or mock the thing under test into meaninglessness.
5. Run the new tests and iterate until they pass against the current code — a generated test that doesn't run is worse than none.
6. Flag any case you couldn't cover (needs a live external service, needs test data you don't have) instead of faking it.

## Guidelines
- Don't delete or rewrite existing passing tests unless asked.
- Don't mock out the exact unit under test.
- If coverage tooling exists in the project, mention how to run it, but don't assume it's installed.
