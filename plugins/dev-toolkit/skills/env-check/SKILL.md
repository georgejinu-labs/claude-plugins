---
name: env-check
description: Compare .env against .env.example (or equivalent) to catch missing, undocumented, or stale environment variables.
disable-model-invocation: true
---

# Env parity check

## Instructions
1. Locate the env files in the current project: `.env`, `.env.local`, `.env.example`/`.env.sample`/`.env.template`, and any per-environment variants.
2. Diff the variable *names* (not values) between the real `.env` and the example/template file.
3. Report:
   - Variables in `.env` but missing from the example — undocumented, new contributors won't know they're needed.
   - Variables in the example but missing from `.env` — likely to crash at runtime; flag any the code reads via a required lookup (`os.environ["X"]`, `process.env.X!`) without a default.
   - Variables that look unused: grep the codebase for each name and flag any with zero references.
4. Never print actual secret values from `.env` in your output — reference by name only.
5. If `.env.example` doesn't exist and `.env` does, offer to create one (names only, placeholder values) rather than doing it silently.

## Guidelines
- Never commit or `git add` an actual `.env` file.
- Treat any value that looks like a real key/token/password as sensitive — redact it in output even if asked to "show me the .env".
