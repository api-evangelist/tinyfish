---
name: Run automations at scale (batch)
description: Submit many TinyFish Web Agent runs in one request, poll them together, and cancel in bulk.
api: openapi/tinyfish-main-openapi.json
operations: [runBatch, batchGetRuns, batchCancelRuns, listRuns]
---

# Run automations at scale (batch)

## Auth
Send `X-API-Key: <key>`. Base URL `https://agent.tinyfish.ai`.

## Steps
1. **Enqueue:** `POST /v1/automation/run-batch` (`runBatch`) with up to 100 runs. Returns `run_id`s immediately without waiting.
2. **Poll together:** `POST /v1/runs/batch` (`batchGetRuns`) with up to 100 IDs. Returns found runs plus any IDs not found/owned.
3. **Cancel in bulk:** `POST /v1/runs/batch/cancel` (`batchCancelRuns`) with up to 100 IDs. Idempotent — returns cancelled, already-terminal, and not-found IDs.
4. **Browse history:** `GET /v1/runs` (`listRuns`) with `status`, goal text, date filters; cursor-paginated (`cursor`, `limit` up to 100, newest first).

## Rules
- Max 100 runs/IDs per batch request.
- Batch creation is all-or-nothing on validation; it is NOT idempotent, so track your own submission keys.
- Batch get/cancel ARE idempotent by ID.
