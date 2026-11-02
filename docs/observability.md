# Observability: logging, telemetry and debugging

MealCart runs on one machine for one user, so observability is a local SQLite database, a live event
stream to the page, and the server log. Nothing is sent to an external service.

## What is recorded

| Signal | Where | Contents |
|---|---|---|
| Every AI call | table `llm_calls` | run, purpose (plan, repair, swap, intake, store, pick), model, input and output tokens, cost in USD, **latency in ms**, time |
| Run outcome | table `runs` | status (`running`, `awaiting_hitl`, `cart_ready`, `needs_attention`, `failed`), error text, running AI cost, plan, grocery list, cart report |
| Human decisions | table `hitl_events` | kind, open or resolved, times. **Answers are never stored** (codes, passwords) |
| Live progress | event bus, WebSocket, replayed after a reconnect | One line per step. AI events name the model, why it was skipped or switched, and when a backup was asked |
| Per-run usage | `GET /api/runs/{id}` field `llm_usage` | calls, tokens, cost, and per model: calls, tokens, average latency |
| Server errors | uvicorn log | `log.exception` when a run fails; model skips and quota warnings |
| Model choice | `prompts/model_settings.yaml` | Order, timeouts, hedge and stall limits, call cap |

## What each number means

- **Latency** is measured from sending a request to receiving the last token of the winning answer
  (`planner/llm.py`, `_call_once`). A slow model that loses a hedge is cancelled and not logged.
- **Cost** is tokens times the per-million price. All models in the default setup are free tier, so cost is 0
  and **tokens are the budget**: free tiers cap requests or tokens per day.
- **Call cap**: one run may make 40 AI calls (a normal run makes 5 to 10). Reaching it stops the run with
  a message instead of silently using up the day's quota.

## Debugging recipes

```bash
sqlite3 -readonly app/mealcart.db
```

```sql
-- Which models answered the latest run, and how fast?
SELECT model, purpose, input_tokens, output_tokens, latency_ms
FROM llm_calls WHERE run_id = (SELECT id FROM runs ORDER BY created_at DESC LIMIT 1)
ORDER BY created_at;

-- Slowest models over all runs
SELECT model, COUNT(*) calls, ROUND(AVG(latency_ms)/1000.0, 1) avg_s, MAX(latency_ms)/1000 max_s
FROM llm_calls WHERE latency_ms IS NOT NULL GROUP BY model ORDER BY avg_s DESC;

-- Why did a run fail, and is any run stuck?
SELECT id, status, error, llm_cost_usd, updated_at FROM runs ORDER BY created_at DESC LIMIT 10;
SELECT run_id, kind, status FROM hitl_events WHERE status = 'open';
```

| Symptom | Where to look |
|---|---|
| Planning is slow | Activity: "X is slow to answer; also asking Y". Then `llm_calls.latency_ms` per model |
| Run says the AI is out of quota | Activity names each model skipped and why (429, 402, 413, unusable JSON) |
| Items flagged unavailable | Activity "Added at {store}" lines; the picks reply and candidates in the shopper step |
| Page shows nothing after a refresh | The WebSocket replays all events with `?since=0`; check the server log |
| Cart total looks wrong | Cart report: need, have and price per line, per-store subtotal |

## Limits

There are no dashboards, alerts or traces. The log is plain, and old databases from before the latency
column have empty latency for earlier calls (the column is added at startup). A multi-user deployment
would need structured logs and per-user cost tracking.
