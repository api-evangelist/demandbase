# Demandbase GraphQL Schema

## Overview

This conceptual GraphQL schema represents the Demandbase ABM (Account-Based Marketing) and B2B intelligence platform. Demandbase provides account identification, intent data, advertising management, engagement analytics, company/contact intelligence, and CRM integrations for B2B go-to-market teams.

The schema is derived from the Demandbase REST APIs documented at https://docs.demandbase.com/ and reflects the platform's core data domains: account intelligence, intent signals, advertising campaigns, engagement tracking, company firmographics/technographics, contact data, and platform administration.

## Schema Source

- **Provider:** Demandbase
- **Documentation:** https://docs.demandbase.com/
- **KB / Support:** https://kb.demandbase.com/hc/en-us/categories/6520773158171-API
- **Schema Type:** Conceptual (derived from REST APIs)
- **Schema File:** demandbase-schema.graphql

## Type Categories

### Account Intelligence (6 types)

| Type | Description |
|------|-------------|
| `AccountId` | Unique identifier reference holding Demandbase, domain, and CRM IDs |
| `Account` | Core account record with firmographic attributes and nested relations |
| `AccountDetails` | Enriched firmographic and enrichment data beyond core fields |
| `AccountScore` | Predictive pipeline, engagement, fit, and intent scores |
| `AccountProfile` | ICP alignment metrics including firmographic and technographic fit |
| `AccountHealth` | Renewal risk, churn probability, product adoption, and health scores |

### Intent Data (3 types)

| Type | Description |
|------|-------------|
| `Intent` | Aggregated intent for an account with stage classification and top keywords |
| `IntentDetails` | Keyword-level intent signal with source sites and research volume |
| `IntentScore` | Numeric score with trend, change delta, and percent change |

### Segments & Audiences (6 types)

| Type | Description |
|------|-------------|
| `Segment` | Named account grouping based on filter criteria |
| `SegmentDetails` | Segment configuration, filter rules, and refresh settings |
| `SegmentMember` | An account's membership record within a segment |
| `Audience` | Targetable audience for advertising or personalization |
| `AudienceDetails` | Audience configuration, bid strategy, and geo/device targeting |
| `AudienceTarget` | Individual targeting specification within an audience |

### Advertising (7 types)

| Type | Description |
|------|-------------|
| `Campaign` | Top-level advertising campaign with budget, dates, and status |
| `CampaignDetails` | Performance metrics, pacing, and bid strategy details |
| `CampaignType` | Enum: DISPLAY, RETARGETING, ACCOUNT_BASED, CONTENT_SYNDICATION, etc. |
| `CampaignStatus` | Enum: DRAFT, PENDING_REVIEW, ACTIVE, PAUSED, ENDED, ARCHIVED |
| `AdGroup` | Grouping of ads sharing targeting settings within a campaign |
| `Ad` | Individual creative unit with format, size, and click URL |
| `AdDetails` | Creative content, performance metrics, and quality score |

### Targeting (2 types)

| Type | Description |
|------|-------------|
| `Target` | A targeting criterion (account list, keyword, firmographic) applied to an ad group |
| `TargetDetails` | Match type, estimated reach, bid adjustment, and exclusion flag |

### Performance Events (3 types)

| Type | Description |
|------|-------------|
| `Impression` | Single ad impression event with account attribution and cost |
| `Click` | Single ad click with landing page, attribution, and cost |
| `Visit` | Website visit session with duration, page count, and entry/exit pages |

### Engagement (2 types)

| Type | Description |
|------|-------------|
| `Engagement` | Aggregated engagement record across web, ad, and email channels |
| `EngagementDetails` | Channel-specific event type, score, count, and asset reference |

### Company Intelligence (5 types)

| Type | Description |
|------|-------------|
| `Company` | B2B company record in the Demandbase data graph |
| `CompanyDetails` | Extended profile including aliases, subsidiaries, social profiles |
| `CompanyFirmographic` | Structural attributes: employee range, ownership type, growth rate |
| `CompanyTechnographic` | Technology stack with primary CRM, marketing, ERP, and cloud providers |
| `CompanyKeyword` | Keyword/topic associations with relevance score and category |

### Technology (2 types)

| Type | Description |
|------|-------------|
| `Technology` | A technology product used by a company with category and confidence |
| `TechCategory` | Enum covering CRM, MARKETING_AUTOMATION, ERP, ANALYTICS, SECURITY, etc. |

### News (2 types)

| Type | Description |
|------|-------------|
| `News` | News article or press mention related to a company |
| `NewsDetails` | Categories, keywords, entities, author, and social share count |

### Funding (2 types)

| Type | Description |
|------|-------------|
| `Funding` | A funding event with round type, amount, and investor list |
| `FundingDetails` | Valuation, equity percentage, and debt component details |

### Contacts & People (4 types)

| Type | Description |
|------|-------------|
| `Contact` | B2B contact record with role, title, department, and verification status |
| `ContactDetails` | Alternate emails/phones, work history, education, and skills |
| `ContactProfile` | Engagement score, buying propensity, influencer score, opt-in status |
| `ContactRole` | Enum: DECISION_MAKER, INFLUENCER, CHAMPION, ECONOMIC_BUYER, etc. |

### Leads (2 types)

| Type | Description |
|------|-------------|
| `Lead` | Marketing or sales lead linked to a contact and account |
| `LeadDetails` | Source, campaign attribution, qualification notes, and follow-up date |

### Web Activity & Conversations (3 types)

| Type | Description |
|------|-------------|
| `Conversation` | Recorded interaction or conversation tied to an account or contact |
| `PageView` | Single web page view event with device, browser, and geo data |
| `VisitorDetails` | Visitor-level identity, ISP, device, and historical visit metrics |

### CRM Integrations (3 types)

| Type | Description |
|------|-------------|
| `SalesforceIntegration` | Salesforce org connection, sync status, and field mappings |
| `HubSpotIntegration` | HubSpot portal connection, sync status, and object mappings |
| `MarketoIntegration` | Marketo instance connection, program sync, and field mappings |

### Authentication & Administration (2 types)

| Type | Description |
|------|-------------|
| `APIKey` | API key record with scopes, expiry, and usage tracking |
| `Token` | OAuth/bearer token with expiry and scope |

## Enums

| Enum | Values |
|------|--------|
| `BuyingStage` | AWARENESS, CONSIDERATION, DECISION, PURCHASE, RETENTION, ADVOCACY |
| `JourneyStage` | EARLY, MIDDLE, LATE, CLOSED_WON, CLOSED_LOST |
| `IntentStage` | NONE, LOW, MEDIUM, HIGH, VERY_HIGH |
| `CampaignType` | DISPLAY, RETARGETING, ACCOUNT_BASED, CONTENT_SYNDICATION, CONNECTED_TV, AUDIO, NATIVE |
| `CampaignStatus` | DRAFT, PENDING_REVIEW, ACTIVE, PAUSED, ENDED, ARCHIVED |
| `TechCategory` | CRM, MARKETING_AUTOMATION, ERP, ANALYTICS, COLLABORATION, SECURITY, CLOUD_INFRASTRUCTURE, etc. |
| `ContactRole` | DECISION_MAKER, INFLUENCER, END_USER, CHAMPION, ECONOMIC_BUYER, TECHNICAL_EVALUATOR, etc. |

## Example Queries

### Fetch an Account with Intent and Score

```graphql
query AccountIntelligence($domain: String!) {
  companyByDomain(domain: $domain) {
    id
    name
    employeeCount
    industry
    firmographic {
      revenueRange
      ownershipType
      annualGrowthRate
    }
    technographic {
      primaryCRM
      primaryMarketing
      cloudProviders
    }
  }
  intentByDomain(domain: $domain) {
    overallIntentScore
    stage
    buyingStage
    topKeywords
    trending
    details {
      keyword
      score {
        current
        change
        trend
      }
      researchVolume
    }
  }
}
```

### List Active Campaigns with Performance

```graphql
query ActiveCampaigns {
  campaigns(status: ACTIVE) {
    id
    name
    type
    budget
    spend
    startDate
    endDate
    details {
      impressionCount
      clickCount
      actualCTR
      accountsReached
      pacing
    }
    adGroups {
      id
      name
      ads {
        id
        name
        format
        details {
          impressionCount
          clickCount
          ctr
        }
      }
    }
  }
}
```

### Contacts at a Target Account

```graphql
query AccountContacts($accountId: ID!) {
  contacts(accountId: $accountId) {
    id
    firstName
    lastName
    title
    role
    department
    email
    profile {
      engagementScore
      buyingPropensity
      influencerScore
      lastEngagedAt
    }
  }
}
```

### Create a Segment and Campaign

```graphql
mutation SetupABMCampaign {
  seg: createSegment(
    name: "High-Intent Enterprise"
    description: "Accounts with high intent scores in enterprise segment"
    filterCriteria: { intentStage: "HIGH", employeeRange: "1000+" }
  ) {
    id
    name
    accountCount
  }

  campaign: createCampaign(
    name: "Q3 Enterprise Awareness"
    type: DISPLAY
    budget: 50000.00
    startDate: "2026-07-01T00:00:00Z"
    endDate: "2026-09-30T23:59:59Z"
  ) {
    id
    name
    status
    budget
  }
}
```

## Related Resources

- REST API Documentation: https://docs.demandbase.com/
- Authentication Guide: https://docs.demandbase.com/docs/authentication
- Getting Started: https://docs.demandbase.com/docs/getting-started
- Rate Limits: https://docs.demandbase.com/docs/api-rate-limits
- Knowledge Base: https://kb.demandbase.com/hc/en-us/categories/6520773158171-API
- Status Page: https://status.demandbase.com/
- Developer Portal: https://developer.demandbase.com
