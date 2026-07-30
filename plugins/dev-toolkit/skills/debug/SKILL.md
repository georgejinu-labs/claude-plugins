---
name: debug
description: Systematically investigate a bug, crash, or unexpected behavior instead of guessing at a fix — reproduce, isolate the cause, verify before declaring done.
disable-model-invocation: true
---

# Debug

## Instructions
1. Restate the reported symptom in one sentence: what's expected vs. what actually happens.
2. Reproduce it first. If you can't reproduce it, say so explicitly and ask for exact repro steps rather than guessing.
3. Isolate: narrow down to the smallest failing case — which input, which code path, which commit if it's a regression (bisect-style thinking, without necessarily running `git bisect`).
4. Form 2-3 concrete hypotheses ranked by likelihood before touching code. State the evidence for each.
5. Test the top hypothesis with the smallest possible check (a log line, a targeted print/test) — don't jump straight to a speculative fix.
6. Once the root cause is confirmed, propose the fix and explain why it addresses the root cause, not just the symptom.
7. After fixing, re-run the original repro to confirm it's resolved, and check other call sites for the same bug pattern.

## Guidelines
- Never say "this should fix it" without having confirmed the root cause first.
- If the bug is in a dependency or environment, say so rather than papering over it in application code.
- Prefer the smallest fix that addresses the root cause over a broad rewrite.
