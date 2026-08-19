---
name: Subscribe to Demandbase company and person changes
description: >-
  Stand up a verified webhook, create a Demandbase change subscription for companies,
  company news, corporate family trees or people, and consume the resulting alerts — by
  push or by poll.
api: openapi/demandbase-b2b-openapi.yml
operations:
  - updateSubscription_1
  - updateSubscription
  - getSubscriptionJobStatus
  - getSubscriptionDetails
  - getSubscriptionAlertsList
  - getSubscriptionAlertDetails
  - getSubscriptionEntityDetails
  - getAllSubscriptionsByClientID
  - deleteSubscription
generated: '2026-08-13'
method: generated
source: >-
  Grounded in openapi/demandbase-b2b-openapi.yml plus
  asyncapi/demandbase-webhooks.yml and conventions/demandbase-conventions.yml
---

# Subscribe to company and person changes

Base URL: `https://uapi.demandbase.com/data/b2b/v1`.

## 1. Prepare the webhook first

Your callback host must be live **before** you create the subscription.

- It must answer both `HEAD` and `POST`.
- Demandbase sends a `HEAD` request carrying the request header
  `X-DemandbaseAPI-ValidationCode`. Echo the identical value back as a response header of
  the same name.
- Validation may lag subscription creation by up to **2-3 minutes**.

Store the signing secret you supply on the subscription and verify it on inbound calls.

## 2. Create the subscription — `updateSubscription_1`

`POST /subscriptions/job` with a `CreateSubscriptionJobRequestDTO`. Pick a
`subscriptionType`:

| Type | Fires on | Alert payload |
|---|---|---|
| `company` | Tracked company fields change | Company IDs plus the changed fields |
| `companynews` | New articles for a tracked company | Company IDs and articles with timestamps |
| `companyfamilytree` | Family-tree membership changes | Companies added/removed, plus migration messages if the root was acquired |
| `dbPerson` | Person records change | Person IDs with changed fields and employment-level updates per contact ID |

The call is asynchronous and returns a `jobId`, not a subscription.

## 3. Wait for the subscription to exist — `getSubscriptionJobStatus`

`GET /subscriptions/job/{jobId}`. On success the response carries the `subscriptionId`. On
failure it carries the error detail.

## 4. Confirm the webhook came up — `getSubscriptionDetails`

`GET /subscriptions/{subscriptionId}` and read `webhook.status`:

`VERIFICATION_PENDING` → queued. `VERIFICATION_RUNNING` → in progress. `ACTIVE` → delivering.
`DISABLED` → **validation failed**. Note the trap: subscription creation still *succeeds*
when webhook validation fails. You will get a subscription that never delivers unless you
check this field.

## 5. Consume alerts

Push is not the only option — every alert is readable over REST:

- `getSubscriptionAlertsList` — `GET /subscriptions/{subscriptionId}/alerts`, paginated,
  sorted by `createdAt` descending, returning `alertId` + `createdAt`.
- `getSubscriptionAlertDetails` — `GET /subscriptions/{subscriptionId}/alerts/{alertId}` for
  what actually changed.
- `getSubscriptionEntityDetails` — `GET /subscriptions/{subscriptionId}/entityIds` for the
  tracked entities; up to 5,000 per page.

## 6. Maintain

- `updateSubscription` — `PUT /subscriptions/job`. Requires both `subscriptionId` and
  `subscriptionType`. Supports replacing the entity set wholesale, adding/removing entity IDs
  incrementally, or changing frequency, webhook URL, signing secret, fields or news
  categories. Also asynchronous — returns a `jobId`.
- `getAllSubscriptionsByClientID` — `GET /subscriptions`, filterable by type.
- `subscriptionJobsListFetch` — `GET /subscriptions/jobs`, filterable by period
  (`day`/`month`/`year`), date range, job status and job type.
- `deleteSubscription` — `DELETE /subscriptions/{subscriptionId}`. Permanent; halts all
  future alerts.

## Rules

- Subscription jobs share the **2-concurrent-job** cap with `/job` and `/match/job`.
- No idempotency key: a retried `POST /subscriptions/job` creates a second subscription.
  Poll `getSubscriptionJobStatus` instead of resubmitting.
- Demandbase publishes no AsyncAPI document for this surface; the event shapes are the
  subscription-alert schemas in the B2B OpenAPI.
