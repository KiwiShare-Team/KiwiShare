# API performance smoke evidence

This evidence supports issue #343. It is a small, read-only regression smoke
check for existing core API paths. It is not a capacity test, production SLA,
or claim about maximum supported users.

## Reproduce the check

Use the repository-supported Node 22 runtime and install the locked workspace
dependencies first. Start the API locally, then run:

```bash
pnpm perf:api
```

The default target is `http://127.0.0.1:3000`. To compare against the deployed
Render environment explicitly:

```bash
API_PERF_BASE_URL=https://kiwishare.onrender.com pnpm perf:api
```

The script prints a machine-readable JSON report. Set `API_PERF_OUTPUT` to also
save it to a file. The following environment variables make the test adjustable
without changing the script:

| Variable | Default | Purpose |
| --- | ---: | --- |
| `API_PERF_BASE_URL` | `http://127.0.0.1:3000` | API origin; no trailing `/api` |
| `API_PERF_REQUESTS` | `80` | Measured requests |
| `API_PERF_CONCURRENCY` | `4` | Concurrent workers |
| `API_PERF_WARMUP_REQUESTS` | `8` | Unmeasured warm-up requests |
| `API_PERF_TIMEOUT_MS` | `10000` | Per-request timeout |
| `API_PERF_MAX_P95_MS` | `1500` | Overall p95 smoke threshold |
| `API_PERF_MAX_ERROR_RATE` | `0.01` | Maximum failed-request ratio |

## Fixed request mix

Only idempotent, unauthenticated GET routes are exercised, so the smoke check
does not create accounts, listings, orders, or other data.

| Share | Route | Core behaviour represented |
| ---: | --- | --- |
| 10% | `GET /health` | Service availability |
| 40% | `GET /api/usedItems?limit=12` | Main listing feed |
| 30% | `GET /api/usedItems/discovery-options` | Discovery filters |
| 20% | `GET /api/categories` | Category navigation and item counts |

The mix repeats every ten requests. This makes the distribution deterministic
for the default 80-request run.

## Acceptance thresholds

The initial team smoke gate is:

- overall p95 response latency no greater than 1,500 ms after warm-up;
- error rate no greater than 1% (only HTTP 200 is successful);
- every request must complete within 10 seconds.

These are regression-detection thresholds for the current pilot environment,
not a public performance guarantee. The team should approve them during review
and revise them only with recorded measurements and an explanation. The 1%
limit effectively permits no failure in the default 80-request sample.

## Recorded baseline

The baseline in
[`2026-09-28-render-baseline.json`](./api-performance/2026-09-28-render-baseline.json)
was measured from an Apple M4 development machine in Auckland against the
deployed Render API, using Node 22.23.2 on 28 September 2026 local time (the
report timestamp is UTC).

| Metric | Result | Threshold | Outcome |
| --- | ---: | ---: | --- |
| Requests | 80 | — | 80 succeeded |
| Concurrency | 4 | — | fixed comparison setting |
| Overall p50 | 702.47 ms | — | baseline |
| Overall p95 | 1,013.07 ms | ≤ 1,500 ms | pass |
| Overall p99 | 1,250.50 ms | — | baseline |
| Error rate | 0% | ≤ 1% | pass |
| Throughput | 5.68 requests/s | — | observational only |

Render service tier, network path, database contents, and current load can all
affect results. Compare runs only when the target and configuration match, keep
the raw JSON, and investigate repeated threshold failures before making a
performance claim.

