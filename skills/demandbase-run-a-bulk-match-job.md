---
name: Run a bulk match or bulk retrieval job on Demandbase
description: >-
  Submit an asynchronous Demandbase bulk match or bulk data-retrieval job, poll it to a
  terminal state, and download the signed CSV result before the URL expires.
api: openapi/demandbase-b2b-openapi.yml
operations:
  - match_1
  - match
  - createBulkJob
  - getBulkJobStatus
generated: '2026-08-13'
method: generated
source: >-
  Grounded in openapi/demandbase-b2b-openapi.yml plus
  conventions/demandbase-conventions.yml and rate-limits/demandbase-rate-limits.yml
---

# Run a bulk match or bulk retrieval job

Base URL: `https://uapi.demandbase.com/data/b2b/v1`. Authenticate exactly as in
`demandbase-enrich-company-and-contacts.md`.

## Choose synchronous or asynchronous

- **`match_1`** — `POST /match`. Synchronous batch matching. The body is a `requests` array;
  each object needs a unique `id` and at least one company identifier from `name`,
  `websites`, `email`, `executiveLinkedInHandle`. Use this when the batch is small enough to
  answer in one round trip.
- **`match`** — `POST /match/job`. Asynchronous CSV enrichment, up to **10,000 records** per
  job. Use this for files.
- **`createBulkJob`** — `POST /job`. Asynchronous company or contact retrieval by entity ID
  or search filter, up to **10,000 entity IDs** per request.

## Submit

Both async endpoints return `202` with a `jobId`. Record it immediately — there is no
idempotency key, so a retried submit creates a **second** job and burns a second slot
against the concurrency cap.

## Poll — `getBulkJobStatus`

`GET /job/{jobId}` works for every bulk job type (Company Fetch, Contact Fetch, Match).

- Non-terminal states: `accepted`, `processing` — keep polling.
- Terminal states: `finished`, `failed`.
- `resultsUrl` is present **only** when status is `finished`, and the signed URL is valid
  for **24 hours**.
- A failed job carries `errorMessage` and produces no result file.

## Read the result

The `resultsUrl` points at a CSV.

- Column headers match the field names you requested.
- Array fields such as `titles` and `jobFunctions` are pipe-separated (`VP|Director|Manager`).
- For `match`, your input columns are preserved and matched company/contact columns are
  appended (`companyId`, `name`, `city`, `matchScore`, `contactId`, `firstName`, `title`,
  `email`, …). Multiple matches become multiple rows sharing one input `id`. No match leaves
  the match columns empty rather than dropping the row.

## Rules

- **Concurrency cap: 2.** At most two jobs may be in `accepted` or `processing` at once
  across `/job`, `/match/job` and `/subscriptions/job`. A third submit returns `429` with
  "Max 2 jobs are allowed at a time". Queue client-side.
- **1,000 requests/minute** across the whole B2B API, including your polling. Poll with
  backoff, not in a tight loop.
- **No `Retry-After`.** Choose your own backoff.
- Download inside the 24-hour window or re-run the job.
