# Bloomreach (bloomreach)

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
