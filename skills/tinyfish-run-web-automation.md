---
name: Run a web automation goal
description: Drive the TinyFish Web Agent to complete a natural-language goal on a live website and return structured results.
api: openapi/tinyfish-main-openapi.json
operations: [run, runAsync, getRunById, cancelRunById]
---

# Run a web automation goal

Automate a task on a real website with a natural-language goal.

## Auth
Send `X-API-Key: <key>` (from agent.tinyfish.ai/api-keys) on every request. Base URL `https://agent.tinyfish.ai`.

## Steps
1. **Short task (wait for result):** `POST /v1/automation/run` (`run`) with `{ goal, url, output_schema? }`. Returns the completed run once finished. Note: sync runs cannot be cancelled.
2. **Long task (don't block):** `POST /v1/automation/run-async` (`runAsync`) with the same body. Returns a `run_id` immediately.
3. **Poll:** `GET /v1/runs/{id}` (`getRunById`) until `status` is a terminal state (`COMPLETED`, `FAILED`, `CANCELLED`). Read `result` and `steps`.
4. **Cancel if needed:** `POST /v1/runs/{id}/cancel` (`cancelRunById`) — only async/SSE runs are cancellable; this call is idempotent.

## Rules
- Pass `output_schema` to get structured JSON instead of free text.
- Pass an HTTPS `webhook_url` to receive `run.completed`/`run.failed`/`run.cancelled` instead of polling (see asyncapi/tinyfish-agent-webhooks.yml).
- Run creation is NOT idempotent — do not blindly retry a failed create or you may spawn duplicate runs (see conventions/tinyfish-conventions.yml).
- Handle run-outcome codes `TASK_FAILED`, `MAX_STEPS_EXCEEDED`, `SITE_BLOCKED`, `CONTENT_POLICY_VIOLATION` (errors/tinyfish-error-codes.yml).
