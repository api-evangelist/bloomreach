---
name: bloomreach-product-search
description: Run a Bloomreach Discovery product and category search, read the response envelope, and page through results correctly.
api: openapi/bloomreach-product-category-search-api-v1-api-openapi.yml
operations:
  - product-category-search-api
  - content-search-api
  - bestseller-api
generated: '2026-08-13'
method: generated
source: openapi/bloomreach-product-category-search-api-v1-api-openapi.yml + https://documentation.bloomreach.com/discovery/reference/welcome
---

# Search a Bloomreach Discovery catalog

## Before you call anything

Discovery search is **not** OAuth. It authenticates with query parameters, not a header:

- `account_id` — your Bloomreach account identifier
- `auth_key` — the Discovery auth key for that account
- `domain_key` — the site/catalog you are searching

Base host is `https://core.dxpapi.com/api/v1/core`. Do **not** send requests to
`api.bloomreach.com` — that hostname does not resolve. See
`conventions/bloomreach-conventions.yml`.

## The happy path

1. Call `product-category-search-api` (`GET /` on the core host) with:
   - `request_type=search` and `search_type=keyword` for a keyword search, or
     `search_type=category` with `q` set to the category id for a category listing
   - `q` — the query string
   - `fl` — the field list you want back. Ask for the fields you will render and
     nothing else; this is the only response-shaping control the API offers.
   - `start` and `rows` — offset pagination. `start` is a record offset, not a page number.
2. Read the response envelope: `response.numFound` is the total, `response.start`
   echoes your offset, `response.docs` is the array of product documents, and
   `facet_counts` carries the facets.
3. Page by incrementing `start` by `rows`. There is no cursor and no
   `next` link — if the catalog changes mid-scan you can see the same document twice.

## Sibling operations

- `content-search-api` — same shape, searches editorial/content items instead of products.
- `bestseller-api` — ranked bestseller list for a category or the whole catalog.
- `autosuggest-api` (in `openapi/bloomreach-autosuggest-api-v2-api-openapi.yml`) lives on a
  **different host**, `https://suggest.dxpapi.com/api/v2/suggest`. Do not assume one base URL
  for all of Discovery.

## Errors and limits

- `400` — bad or missing query parameters. The spec's description is literally
  "If the query parameters are incorrect/invalid"; there is no field-level error detail.
- `401` — wrong `auth_key` or an `account_id` it does not belong to.
- `429` — rate limited. Honour `Retry-After` and back off exponentially.
  Discovery limits are set per customer; see `rate-limits/bloomreach-rate-limits.yml`.
- Responses are **not** RFC 9457 problem+json. Parse the product-specific JSON body
  described in `errors/bloomreach-problem-types.yml`.

## Do not

- Do not retry a failed search by changing `rows` — the API is read-only here, so retrying
  the identical request is safe, but there is no idempotency key anywhere in Bloomreach,
  so never carry this habit over to a write call.
