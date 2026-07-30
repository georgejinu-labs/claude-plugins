---
name: sql-review
description: Review a SQL query (especially BigQuery) for performance and cost issues before it runs — full scans, missing partition/cluster filters, unnecessary SELECT *, accidental cross joins.
disable-model-invocation: true
---

# SQL review

## Instructions
1. Identify the query — from the message, a file, or the most recently edited .sql file / query string in code.
2. Identify the target engine (BigQuery, Postgres, etc.) from context and check engine-specific concerns:
   - BigQuery: missing partition filter (`_PARTITIONTIME`/date column) or clustering column in WHERE, `SELECT *` on wide tables, unbounded JOIN fan-out, `CROSS JOIN` where a keyed `INNER JOIN` was meant, repeated subqueries that should be a CTE, wildcard table scans (`table_*`) without a date range.
   - Postgres/MySQL: missing index for filtered/joined columns (check against known schema if available), N+1 query patterns where application code calls this query in a loop.
3. Estimate cost/impact qualitatively — for BigQuery, mention that `bq query --dry_run` (or `EXPLAIN`) gives an exact bytes-scanned number, and suggest running it before executing against a large dataset.
4. Rewrite the query only if asked; otherwise report the specific line/clause and the concrete fix.
5. Never execute a query against a live database/warehouse yourself unless explicitly asked — reviewing is read-only.

## Guidelines
- Don't flag stylistic-only issues (aliasing, formatting) as performance problems.
- If the table size/partitioning scheme is unknown, ask rather than assuming a fix is needed.
