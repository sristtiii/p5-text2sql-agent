# P5 Progress

## How we work
1. Do only the **current** module.
2. Run its check, then ask for a review.
3. The module is ticked only after the code is reviewed and the check passes.
4. Commit and push, then move to the next one.

**Current module: 1 (`scripts/seed_db.py`)**

## Stage 1: Baseline

- [ ] **1. `scripts/seed_db.py`**: build `data/shop.db`
  - Tables `customers`, `orders`, `order_items` with primary and foreign keys
  - Rows small enough to check by hand, including a customer with no orders
  - Foreign keys enforced
  - Check: `python scripts/seed_db.py` twice, with no error and no duplicate rows
- [ ] **2. `app/config.py`** (and `.env.example`): load and validate settings
  - Check: a missing required setting gives a clear error
- [ ] **3. `app/db.py`**: open the database and run a query
  - Returns column names and rows, and closes the connection
  - Check: a query on `shop.db` returns the expected rows
- [ ] **4. `tests/test_db.py`**: tests for `app/db.py`
  - Uses a temporary database; covers a normal query, no rows and bad SQL
  - Check: `python -m pytest` passes
- [ ] **5. `app/schema.py`**: read tables, columns and keys; render as text
  - Check: output for `shop.db` shows every table, column and foreign key
- [ ] **6. `app/prompt.py`**: build the prompt from question and schema
  - Check: the printed prompt contains the question, schema and output rules
- [ ] **7. `app/llm.py`**: call the model (choose the provider first)
  - Handles timeouts, errors and extracting SQL from the reply
  - Check: one real call returns SQL
- [ ] **8. `app/pipeline.py`**: question to SQL to rows
  - Check: tested with a fake model, so no real call is needed
- [ ] **9. `app/main.py`**: command-line entry point
  - Opens the database read-only until the guardrails exist
  - Check: ask a question and verify the rows by hand
- [ ] **10. `README.md`**: setup and run steps
  - Check: the steps work as written

## Stage 2: Guardrails

- [ ] **11. `app/guardrails/policy.py`**: allowed tables and columns, row and time limits
- [ ] **12. `app/guardrails/input_guard.py`**: check the question before the model sees it
- [ ] **13. `app/guardrails/sql_guard.py`**: parse SQL, allow reads only, enforce allowlist and limits
- [ ] **14. `tests/test_sql_guard.py`**: unsafe queries rejected, safe queries allowed
- [ ] **15. Wire guardrails into `db.py` and `pipeline.py`**: read-only connection and query timeout

## Stage 3: Semantic cache

- [ ] **16. `app/cache/normalizer.py`**: canonical form of a question
- [ ] **17. `app/cache/embedder.py`**: text to vector
- [ ] **18. `app/cache/store.py`**: save and load entries, TTL, schema version
- [ ] **19. `app/cache/lookup.py`**: similarity search; hit, miss or bypass
- [ ] **20. `app/cache/telemetry.py`**: log cache events and timings
- [ ] **21. `tests/test_cache.py`**: expiry, schema change, similar but different questions
- [ ] **22. Wire the cache into `pipeline.py`**: cached SQL still passes the guardrails

## Stage 4: Evaluation

- [ ] **23. `eval/dataset.py`**: load Spider or your own test cases
- [ ] **24. `eval/compare.py`**: decide if two result sets match
- [ ] **25. `tests/test_compare.py`**: order, duplicates, NULLs and numbers
- [ ] **26. `eval/errors.py`**: categorize failures
- [ ] **27. `eval/runner.py`**: run cases and log raw results
- [ ] **28. `eval/report.py`**: summarize only what is in the logs
- [ ] **29. `README.md`**: add results you measured yourself
