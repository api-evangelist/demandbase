# Demandbase (demandbase)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Demandbase is a B2B go-to-market platform that unifies account intelligence, intent data, advertising, orchestration, personalization and sales intelligence into a single pipeline engine. Its developer surface is eight OpenAPI-documented REST APIs on uapi.demandbase.com — B2B company/contact intelligence, Data Export, Data Import, Admin, Intent (beta), Usage, Custom Sources and Auth — plus a hosted, OAuth-protected Model Context Protocol server that exposes account, person, intent, buying-group and account-brief capabilities to AI assistants, an official Python SDK, and a change-subscription webhook surface.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/demandbase/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/demandbase/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Position:** Consumer
- **Access:** 3rd-Party

## Tags

- Account-Based Marketing
- Advertising
- AI Agents
- B2B Marketing
- Company Data
- Contact Data
- Data Enrichment
- Intent Data
- MCP
- Personalization
- Sales Intelligence
- Technographics

## APIs

### Demandbase B2B API

Company and contact intelligence: search and fetch companies and contacts, company news, logos, installed technologies and corporate hierarchy; synchronous and bulk matching; asynchronous bulk data retrieval; and change subscriptions with webhook alerts for company, company-news, family-tree and person records. 22 operations.

- **Human URL:** https://developer.demandbase.com/docs/b2b-overview
- **Base URL:** `https://uapi.demandbase.com/data/b2b/v1`
- **OpenAPI:** openapi/demandbase-b2b-openapi.yml
- **Documentation:** https://developer.demandbase.com/docs/b2b-overview
- **APIReference:** https://developer.demandbase.com/reference/companysearch_1
- **GettingStarted:** https://developer.demandbase.com/docs/b2b-make-your-first-request
- **Authentication:** https://developer.demandbase.com/docs/b2b-authentication
- **RateLimits:** https://developer.demandbase.com/docs/b2b-rate-limits
- **Webhooks:** asyncapi/demandbase-webhooks.yml
- **Overlay:** overlays/demandbase-b2b-overlay.yaml
- **BestPractices:** https://developer.demandbase.com/docs/b2b-best-practices
- **FAQ:** https://developer.demandbase.com/docs/faq

### Demandbase Data Export API

Asynchronous export of Demandbase platform data — Account, Opportunity, Person, Activity, Campaign, Creative, Account List and Person List entities — to CSV or JSON behind a signed download URL. Field availability is governed by the tenant Export Collection. 8 operations.

- **Human URL:** https://developer.demandbase.com/docs/export-overview
- **Base URL:** `https://uapi.demandbase.com/data/export`
- **OpenAPI:** openapi/demandbase-data-export-openapi.yml
- **Documentation:** https://developer.demandbase.com/docs/export-overview
- **APIReference:** https://developer.demandbase.com/reference/createexportjob
- **GettingStarted:** https://developer.demandbase.com/docs/make-your-first-request
- **Authentication:** https://developer.demandbase.com/docs/authentication
- **RateLimits:** https://developer.demandbase.com/docs/rate-limits
- **ErrorCatalog:** https://developer.demandbase.com/docs/handling-errors
- **Overlay:** overlays/demandbase-data-export-overlay.yaml
- **Filters:** https://developer.demandbase.com/docs/how-to-use-filters

### Demandbase Data Import API

Asynchronous import of customer data and intent activity into the Demandbase platform. Create an import job, submit a data file of up to 5 GB, poll the job, and manage custom activity types and CSV record matching / data mapping. 9 operations.

- **Human URL:** https://developer.demandbase.com/docs/import-overview
- **Base URL:** `https://uapi.demandbase.com/import/v1`
- **OpenAPI:** openapi/demandbase-data-import-openapi.yml
- **Documentation:** https://developer.demandbase.com/docs/import-overview
- **APIReference:** https://developer.demandbase.com/reference/post_job
- **GettingStarted:** https://developer.demandbase.com/docs/make-your-first-request-1
- **Authentication:** https://developer.demandbase.com/docs/authentication-1
- **RateLimits:** https://developer.demandbase.com/docs/rate-limits-1
- **Overlay:** overlays/demandbase-data-import-overlay.yaml

### Demandbase Intent API

Beta. Company-level intent signals and research activity: query by company IDs, keyword set IDs or keywords over a date range, filtered by intent strength or number of people researching, with cursor-based pagination. 1 operation.

- **Human URL:** https://developer.demandbase.com/reference/company-intent
- **Base URL:** `https://uapi.demandbase.com/data/intent/v1`
- **OpenAPI:** openapi/demandbase-intent-openapi.yml
- **Documentation:** https://developer.demandbase.com/reference/company-intent
- **APIReference:** https://developer.demandbase.com/reference/companyintent-1
- **ChangeLog:** https://developer.demandbase.com/changelog/new-beta-company-intent-api
- **Overlay:** overlays/demandbase-intent-overlay.yaml

### Demandbase Admin API

User administration for a Demandbase tenant: create, update, retrieve, list and delete users, with filters for departments, views, permission sets and workspaces. 5 operations.

- **Human URL:** https://developer.demandbase.com/docs/user-admin-overview
- **Base URL:** `https://uapi.demandbase.com`
- **OpenAPI:** openapi/demandbase-admin-openapi.yml
- **Documentation:** https://developer.demandbase.com/docs/user-admin-overview
- **APIReference:** https://developer.demandbase.com/reference/post_admin-v1-user
- **Authentication:** https://developer.demandbase.com/docs/authentication-3
- **Overlay:** overlays/demandbase-admin-overlay.yaml

### Demandbase Usage API

Credit usage reporting: returns a summary of credit consumption and entitlements for a given API category, the only runtime signal a consumer has for remaining quota. 1 operation.

- **Human URL:** https://developer.demandbase.com/docs/credit-usage-overview
- **Base URL:** `https://uapi.demandbase.com`
- **OpenAPI:** openapi/demandbase-usage-openapi.yml
- **Documentation:** https://developer.demandbase.com/docs/credit-usage-overview
- **APIReference:** https://developer.demandbase.com/reference/getcreditusagedetails
- **Authentication:** https://developer.demandbase.com/docs/authentication-2
- **Overlay:** overlays/demandbase-usage-overlay.yaml

### Demandbase Custom Sources API

Manage custom data sources and their per-object field mappings so third-party systems can feed the Demandbase platform. 7 operations.

- **Human URL:** https://developer.demandbase.com/docs/custom-sources-overview
- **Base URL:** `https://uapi.demandbase.com/integration/v1`
- **OpenAPI:** openapi/demandbase-custom-sources-openapi.yml
- **Documentation:** https://developer.demandbase.com/docs/custom-sources-overview
- **APIReference:** https://developer.demandbase.com/reference/get_integration-v1-custom-sources
- **Overlay:** overlays/demandbase-custom-sources-overlay.yaml

### Demandbase Auth API

Token exchange for every other Demandbase API: POST an API Key Set client ID and client secret with grantType client_credentials and receive a bearer access token valid for 8 hours. 1 operation.

- **Human URL:** https://developer.demandbase.com/docs/authenticating-with-the-apis
- **Base URL:** `https://uapi.demandbase.com`
- **OpenAPI:** openapi/demandbase-auth-openapi.yml
- **Documentation:** https://developer.demandbase.com/docs/authenticating-with-the-apis
- **APIReference:** https://developer.demandbase.com/reference/generate_access_token
- **Authentication:** authentication/demandbase-authentication.yml
- **Overlay:** overlays/demandbase-auth-overlay.yaml

### Demandbase MCP Server

Hosted remote Model Context Protocol server exposing Demandbase account, person, intent, buying-group, account-brief, global company/contact and reference capabilities to AI assistants. OAuth 2.1 authorization-code + PKCE with dynamic client registration; tools/list is tenant-gated.

- **Human URL:** https://developer.demandbase.com/docs/mcp
- **Base URL:** `https://gateway.demandbase.com/mcp/servers/db-mcp`
- **MCPServer:** mcp/demandbase-mcp.yml
- **ToolCrosswalk:** mcp/demandbase-tool-crosswalk.yml
- **Documentation:** https://developer.demandbase.com/docs/mcp
- **GettingStarted:** https://developer.demandbase.com/docs/custom-mcp-clients
- **Authentication:** authentication/demandbase-authentication.yml
- **OAuthScopes:** scopes/demandbase-scopes.yml

### Demandbase IP-API v3

Real-time visitor identification: resolves a visitor IP address to a Demandbase company ID, firmographics and corporate hierarchy for web personalization, forms enrichment and analytics integrations. No OpenAPI is published for this API and its reference documentation sits on the support knowledge base, which is behind a bot challenge.

- **Human URL:** https://support.demandbase.com/hc/en-us/articles/23789223879323-Demandbase-IP-API-v3-for-Demandbase-One-Current-Version
- **Base URL:** `https://api.company-target.com`
- **Documentation:** https://support.demandbase.com/hc/en-us/articles/23789223879323-Demandbase-IP-API-v3-for-Demandbase-One-Current-Version
- **Migration:** https://support.demandbase.com/hc/en-us/articles/25137915441947-Upgrading-to-Demandbase-IP-API-v3

## Common properties

- **AgenticAccess:** agentic-access/demandbase-agentic-access.yml
- **TrustCenter:** security/demandbase-trust-center.yml
- **DomainSecurity:** security/demandbase-domain-security.yml
- **Authentication:** authentication/demandbase-authentication.yml
- **GitHubOrganization:** https://github.com/demandbase
- **StatusPage:** https://status.demandbase.com/
- **Support:** https://support.demandbase.com/
- **PrivacyPolicy:** https://www.demandbase.com/privacy-policy/
- **Blog:** https://www.demandbase.com/blog/
- **LinkedIn:** https://www.linkedin.com/company/demandbase/
- **Twitter:** https://twitter.com/Demandbase
- **Portal:** https://developer.demandbase.com
- **KnowledgeBase:** https://kb.demandbase.com/hc/en-us
- **Partners:** https://partners.demandbase.com/
- **Integrations:** https://partners.demandbase.com/t/partners/integrations
- **TermsOfService:** https://www.demandbase.com/terms-of-use/
- **Signup:** https://www.demandbase.com/products/data/api-integration/api-trial/
- **Vocabulary:** vocabulary/demandbase-vocabulary.yml
- **Packages:** packages/demandbase-packages.yml
- **SDKs:** packages/demandbase-packages.yml
- **WellKnown:** well-known/demandbase-well-known.yml
- **MCPServer:** mcp/demandbase-mcp.yml
- **ToolCrosswalk:** mcp/demandbase-tool-crosswalk.yml
- **LLMsTxt:** llms/demandbase-llms.txt
- **Conformance:** conformance/demandbase-conformance.yml
- **Compliance:** https://trust.demandbase.com/
- **ErrorCatalog:** errors/demandbase-error-codes.yml
- **ErrorCodes:** errors/demandbase-problem-types.yml
- **Lifecycle:** lifecycle/demandbase-lifecycle.yml
- **Deprecation:** https://developer.demandbase.com/docs/migrating-from-legacy-tokens-to-api-keysets
- **OAuthScopes:** scopes/demandbase-scopes.yml
- **Conventions:** conventions/demandbase-conventions.yml
- **ChangeLog:** changelog/demandbase-changelog.yml
- **DataModel:** data-model/demandbase-data-model.yml
- **Webhooks:** asyncapi/demandbase-webhooks.yml
- **AgentSkill:** skills/_index.yml
- **RateLimits:** rate-limits/demandbase-rate-limits.yml
- **Plans:** plans/demandbase-plans-pricing.yml
- **FinOps:** finops/demandbase-finops.yml
- **DeveloperPortal:** https://developer.demandbase.com
- **Documentation:** https://developer.demandbase.com/docs/welcome
- **APIReference:** https://developer.demandbase.com/reference/generate_access_token
- **GettingStarted:** https://developer.demandbase.com/docs/api-getting-started
- **Pricing:** https://www.demandbase.com/pricing/
- **SignUp:** https://www.demandbase.com/products/data/api-integration/api-trial/
- **Login:** https://web.demandbase.com/
- **HelpCenter:** https://support.demandbase.com/hc/en-us
- **SDK:** https://pypi.org/project/demandbase-sdk/
- **Contact:** https://www.demandbase.com/company/contact-us/

## Provenance

All eight OpenAPI definitions in `openapi/` were harvested verbatim on 2026-08-13 from the Demandbase developer portal at https://developer.demandbase.com. They replace 28 scaffolded specifications that a prior bulk pass had written against `api.demandbase.com` — a host that returns "404 page not found" on every path. The real production host is `uapi.demandbase.com`. Artifacts derived from the scaffolds (GraphQL schema, Postman/Open collections, Spectral rules, JSON Schema, JSON Structure, JSON-LD context) were removed in the same pass rather than carried forward.
