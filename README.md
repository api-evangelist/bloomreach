# Bloomreach (bloomreach)

Bloomreach is a digital commerce experience platform that unifies real-time customer and product data to power personalized shopping journeys across every channel. The platform provides REST APIs spanning autonomous search and merchandising (Discovery), marketing automation and CDP (Engagement), and headless content management (Content/brXM), along with a Data Hub for unified data operations and an AI agent interface (Loomi Connect).

APIs.json: https://raw.githubusercontent.com/api-evangelist/bloomreach/refs/heads/main/apis.yml

Naftiko: https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=bloomreach-api-evangelist&utm_content=repo

## Tags

Digital Commerce, Search, Merchandising, Recommendations, Customer Data Platform, CDP, Email Marketing, SMS Marketing, Marketing Automation, Headless CMS, Personalization, E-commerce

## APIs

- **Bloomreach Discovery API** — REST APIs for site search, category browsing, autosuggest, recommendations, bestseller listings, and content search. Documentation: https://documentation.bloomreach.com/discovery/reference/welcome
- **Bloomreach Discovery Catalog Management API** — REST API for sending and managing product catalog data, supporting full feed uploads, incremental patch updates, and indexing job triggers. Documentation: https://documentation.bloomreach.com/discovery/reference/catalog-management-api-v3
- **Bloomreach Engagement API** — REST API for the CDP and marketing automation platform enabling customer tracking, segmentation, campaign management, email/SMS sends, recommendations, and analytics. Documentation: https://documentation.bloomreach.com/engagement/reference/about
- **Bloomreach Content Management API** — REST APIs for managing headless CMS content including channels, content types, documents, folders, batch import/export, projects, integrations, and webhooks. Documentation: https://documentation.bloomreach.com/content/reference/management-apis
- **Bloomreach Content Delivery API** — REST endpoints for SPAs to retrieve JSON representations of channels, pages, and documents from the Bloomreach headless CMS. Documentation: https://documentation.bloomreach.com/content/reference/delivery-api
- **Bloomreach Data Hub Workspace Import API** — REST API for importing workspace data into Bloomreach Data Hub for unified data platform operations. Documentation: https://documentation.bloomreach.com/

## Plans / Rate Limits / FinOps

- **Plans**: [plans/bloomreach-plans-pricing.yml](plans/bloomreach-plans-pricing.yml) — Modular custom pricing across Engagement (Essentials from ~$1,585/month, Growth, Enterprise), Discovery, and Content modules. Scaling dimensions: customer count, catalog size, communication send volume, and events.
- **Rate Limits**: [rate-limits/bloomreach-rate-limits.yml](rate-limits/bloomreach-rate-limits.yml) — Catalog Management default 1 req/sec; indexing/full feed 1 per 10 minutes; PATCH 1440/day cap; Engagement Tracking 150 req/sec/IP; 100M event default contract cap. HTTP 429 with Retry-After used across all APIs.
- **FinOps**: [finops/bloomreach-finops.yml](finops/bloomreach-finops.yml) — FOCUS-aligned guidance covering module fees, event usage fees, catalog size scaling, communication volume, right-sizing event tracking, optimizing catalog feed cadence, and modular procurement.

## Timestamps

- Created: 2026-06-13
- Modified: 2026-06-13

## Common

- Website: https://www.bloomreach.com
- Documentation: https://documentation.bloomreach.com/
- GitHub Org: https://github.com/bloomreach
- GitHub API Specs: https://github.com/bloomreach/api-specs
- LinkedIn: https://www.linkedin.com/company/bloomreach
- Blog: https://www.bloomreach.com/en/blog
- Pricing: https://www.bloomreach.com/en/pricing
- Status Page: https://status.bloomreach.com
- X: https://x.com/bloomreach_tm

## Maintainers

- Kin Lane (kin@apievangelist.com)
