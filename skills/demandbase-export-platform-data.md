---
name: Export Demandbase platform data
description: >-
  Create a Demandbase Data Export job for accounts, opportunities, people, activities,
  campaigns, creatives or list membership, poll it to a terminal state, and download the
  result — while staying inside the daily endpoint quotas and the tenant's Export Collection.
api: openapi/demandbase-data-export-openapi.yml
operations:
  - fetchFields
  - createExportJob
  - fetchAccountListJob
  - fetchPersonListJob
  - checkExportJobStatus
  - fetchExportJobsInfo
  - get-reference
  - get-support-object
generated: '2026-08-13'
method: generated
source: >-
  Grounded in openapi/demandbase-data-export-openapi.yml plus
  errors/demandbase-error-codes.yml and rate-limits/demandbase-rate-limits.yml
---

# Export Demandbase platform data

Base URL: `https://uapi.demandbase.com/data/export`. Versions live in the operation paths
(`/v1/fields`, `/v1/job`, …). Authenticate with a bearer token from
`POST https://uapi.demandbase.com/auth/v1/token`.

## 1. Check what you are entitled to — `fetchFields`

`GET /v1/fields` returns the fields available for the export you intend to run. For
`campaign` and `creative` exports the `adReportType` query parameter is **required**.

Field availability is contractual, not just schematic: it depends on the tenant's Export
Collection (1-4). Requesting a field outside the enabled collection returns `400-155` or
`403-104` — not an empty column. Call this first rather than guessing.

`get-reference` (`GET /v1/references`) lists available reference objects and
`get-support-object` (`GET /v1/reference/{object}`) returns the reference values for one.

## 2. Submit the job

- **`createExportJob`** — `POST /v1/job` for Account, Opportunity, Person, Activity,
  Campaign and Creative.
- **`fetchAccountListJob`** — `POST /v1/accountList/job`.
- **`fetchPersonListJob`** — `POST /v1/personList/job`.

Those two routes are **case-sensitive**; Demandbase corrected the documentation for exactly
this on 2026-08-11.

`jobName` is required and must be 50 characters or fewer (`400-126`). Activity date ranges
cannot exceed 90 days (`400-114`). `fromDate` and `toDate` must be supplied together
(`400-112`) and in order (`400-113`/`400-137`).

## 3. Poll — `checkExportJobStatus`

`GET /v1/job/{jobId}`. Non-terminal: `accepted`, `processing`. Terminal: `finished`,
`failed`. A finished job includes its download URL; a failed job does not produce a result
file. Requesting results before `finished` returns `400-117`.

`fetchExportJobsInfo` (`GET /v1/jobs`) lists your submitted jobs.

## Quotas — this API is the tightest surface Demandbase publishes

| Endpoint | Daily limit (resets midnight UTC) |
|---|---|
| `POST /v1/job` | 60 |
| `GET /v1/jobs` | 1,200 |
| `GET /v1/job/{jobId}` | 300 |
| `GET /v1/fields` | 60 |

Plus: **10 GB combined export volume per day**, and **only one export job may run at a
time**. Trial accounts are additionally capped at **100 MB over a rolling 24 hours**, checked
*before* the export runs — an oversized export is rejected outright, so narrow the filters
or select fewer fields.

With 300 status calls per day and one concurrent job, a 30-second poll interval exhausts the
status quota in 2.5 hours. Poll on a back-off, not a fixed short interval.

## Errors

`{errorCode, errorMessage, diagnosticCode}`. Exhaustion codes: `429-100` concurrent query,
`429-101` rolling 24-hour quota, `429-102` concurrent export job. There is no `Retry-After`
header — choose your own backoff. Quote `diagnosticCode` when escalating; never parse it.
