---
name: Enrich a company and its contacts with Demandbase
description: >-
  Authenticate against the Demandbase Auth API, resolve a company from a name or domain,
  pull its full firmographic and technographic profile, then find the right people at it.
api: openapi/demandbase-b2b-openapi.yml
operations:
  - companySearch_1
  - companyFetch
  - contactSearch_1
  - contactFetch
generated: '2026-08-13'
method: generated
source: >-
  Grounded in openapi/demandbase-b2b-openapi.yml (harvested verbatim from
  developer.demandbase.com) plus conventions/demandbase-conventions.yml and
  errors/demandbase-error-codes.yml
---

# Enrich a company and its contacts

Base URL: `https://uapi.demandbase.com/data/b2b/v1`

## 1. Get an access token

`POST https://uapi.demandbase.com/auth/v1/token` with
`{"grantType":"client_credentials","clientId":"…","clientSecret":"…"}`.

Read `accessToken` from the response and send it on every subsequent call as
`Authorization: Bearer <accessToken>`. The published example returns `expiresIn: 28800`
(8 hours) — cache the token and refresh on expiry rather than minting one per request,
because token calls count against the same 1,000 requests-per-minute budget.

Never put the client secret in the `Authorization` header.

## 2. Resolve the company — `companySearch_1`

`POST /companies` with a `CompanySearchRequestDTO` body. Search by name, website, ticker,
location (city/state/country/ZIP), industry classification (NAICS/SIC), employee or revenue
range, business structure or Fortune ranking. You must supply at least one non-pagination
parameter.

Use `page` and `perPage` for paging and `sort` to order results. Take `companyId` off the
best match — every downstream call keys on it.

## 3. Pull the full profile — `companyFetch`

`GET /company/{companyId}`.

Use `companyFields` to request only the fields you need, and `include` to expand related
data: family tree, competitors, installed technologies, logos. Installed technologies come
back under `techUsed`, grouped by category, subcategory and product — with taxonomy IDs and
display names, but **not** confidence, observation date, vendor or product description. If
your use case needs those, this endpoint cannot supply them.

## 4. Find people — `contactSearch_1`

`POST /contacts` with a `ContactSearchRequestDTO` body. Filter on first/last name, email,
location, job title, level and function, plus company-level filters (name, website,
location, industry, size). You can also filter on quality score, email-validation status and
phone requirement.

`perPage` on contact search is capped at 50 and `page` must be >= 1 — exceeding either
returns `400` with `errorCode` `400-119` / `400-121`.

## 5. Fetch one person — `contactFetch`

`GET /contact/{contactId}` returns employment and personal detail: titles, phone numbers,
job levels, salary ranges, social profiles, education history. Narrow the payload with
`contactFields` / `peopleFields`.

## Rules

- **No idempotency.** Demandbase publishes no idempotency key. Never blind-retry a POST after
  a timeout; re-read state first.
- **No rate-limit headers.** You get no `RateLimit-*`, no `X-RateLimit-*` and no
  `Retry-After`. Track your own consumption against 1,000 requests/minute and back off
  exponentially on `429`. Poll `GET /reporting/v1/usage` on the Usage API for credit
  entitlement.
- **Errors** are `{errorCode, errorMessage, diagnosticCode}`. The first three digits of
  `errorCode` are the HTTP status. Log `diagnosticCode`; never parse it.
- **Credits are consumed per call.** Prefer one enriched fetch with the right `include` set
  over several narrow ones.
