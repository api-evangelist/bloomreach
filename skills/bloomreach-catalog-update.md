---
name: bloomreach-catalog-update
description: Update Bloomreach Discovery catalog records safely using the ETag conditional-update contract, then trigger and poll an indexing job.
api: openapi/bloomreach-manage-feed-records-api-openapi.yml
operations:
  - "GET /accounts/{account_name}/catalogs/{catalog_name}/environments/{environment_name}/records"
  - "GET /accounts/{account_name}/catalogs/{catalog_name}/environments/{environment_name}/records/{record_id}"
  - "PATCH /accounts/{account_name}/catalogs/{catalog_name}/environments/{environment_name}/records"
  - "PUT /accounts/{account_name}/catalogs/{catalog_name}/environments/{environment_name}/records"
  - "POST /accounts/{account_name}/catalogs/{catalog_name}/environments/{environment_name}/indexes"
  - "GET /accounts/{account_name}/catalogs/{catalog_name}/environments/{environment_name}/jobs/{job_id}"
generated: '2026-08-13'
method: generated
source: openapi/bloomreach-manage-feed-records-api-openapi.yml, openapi/bloomreach-feed-indexing-api-openapi.yml, openapi/bloomreach-job-processing-api-openapi.yml
---

# Update a Discovery catalog record and reindex

Base host: `https://discovery.bloomreach.com/dataconnect/api/v3`.
Auth: HTTP Basic (`Authorization: Basic base64(APIKeyID:APISecret)`).
Every path is scoped by `account_name`, `catalog_name` and `environment_name` — three path
parameters, in that order, on every operation below.

## Read before you write — this API has no idempotency key

Bloomreach documents **no** `Idempotency-Key` header anywhere. A retried `PATCH` is a second
`PATCH`. What it does give you is optimistic concurrency, and you must use it:

1. `GET .../records/{record_id}` and keep the record's **ETag**.
2. `PATCH .../records` with `if_match_etag` set to that value.
3. If you get `412` — the response description is `if_match_etag field did not match expected
   value` — someone else wrote first. Re-read the record, take the **new** ETag, re-apply your
   change, and retry. Never retry a `412` with the stale ETag.

Use `PUT .../records` only when you intend a full replace. `PATCH` is the partial update.

## Trigger indexing and poll the job

4. `POST .../indexes` to start an indexing job. This is rate limited hard — Catalog Management
   defaults to **1 request per second per account**, and the indexing trigger has its own limit.
   See `rate-limits/bloomreach-rate-limits.yml`.
5. The response gives you a job id. Poll `GET .../jobs/{job_id}` until it completes; list all
   jobs with `GET .../jobs`.
6. On `429`, read `Retry-After` and back off exponentially. Do not tighten the poll loop.

## Inspecting structure

- `GET /accounts/{account_name}/catalogs` — list the catalogs on an account.
- `GET .../configs/LATEST` — the live catalog configuration
  (`openapi/bloomreach-catalog-configuration-api-openapi.yml`).
- `GET .../reserved-attributes` — the attribute names Bloomreach reserves. Check this before
  naming a custom attribute; a collision is rejected at index time, not at write time.
- `GET .../records/{record_id}/variants` and `.../views/{view_id}/variants` — the variant and
  view fan-out. See `data-model/bloomreach-data-model.yml`.

## Errors

- `406 Invalid Content-Type` — send `application/json`.
- `413 Content Too Large` — split the batch.
- `412` — concurrency conflict, handled above.
- Error bodies are product-specific JSON, not `application/problem+json`.
