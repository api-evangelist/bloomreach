---
name: bloomreach-loomi-connect-mcp
description: Connect an agent to the Loomi Connect MCP server, authenticate through Bloomreach SSO, and call its read and write tools within the documented rate limits.
api: mcp/bloomreach-mcp.yml
operations:
  - list_projects
  - get_project_overview
  - list_customers
  - get_customer_properties
  - search_scenarios
  - create_scenario
  - update_scenario
  - delete_scenario
  - search_email_campaigns
  - execute_analytics_eql
  - estimate_eql_cost
generated: '2026-08-13'
method: generated
source: https://documentation.bloomreach.com/loomi-connect/docs/get-started-with-mcp and the Loomi Connect tool reference
---

# Use the Loomi Connect MCP server

Loomi Connect is Bloomreach's agent interface. It is a **remote** MCP server — there is nothing
to install. Pick the endpoint for your project's region; using the wrong region fails auth:

| Region | Endpoint |
|---|---|
| US | `https://us.connect.loomi.ai/mcp` |
| EU | `https://eu.connect.loomi.ai/mcp` |
| UK | `https://uk.connect.loomi.ai/mcp` |
| CA | `https://ca.connect.loomi.ai/mcp` |
| AP | `https://ap.connect.loomi.ai/mcp` |

```shell
claude mcp add loomi-mcp --transport http https://us.connect.loomi.ai/mcp
```

## Authentication

OAuth 2.1 authorization code with S256 PKCE, fronted by Bloomreach single sign-on (OIDC).
You do not start the flow yourself — the first tool call does. The server opens the Bloomreach
login page, you sign in with your normal dashboard credentials, and the session lasts **up to
30 days**. Scopes are `openid profile email` only: they establish identity, they grant nothing.

**What you can actually see is decided by your Bloomreach IAM role, server-side.** Without the
`administration.pii.flag` permission, PII comes back masked as `******`, and the tool cannot
override it. An empty result is usually a permissions result, not a data result.

The server must be enabled for your accounts or projects first — contact
`loomi-connect@bloomreach.com`. Loomi Connect is early access: no SLA, and Bloomreach reserves
the right to change or withdraw functionality.

## Anchor the session before you do anything else

Every tool takes a `project_id`. Resolve it once:

1. `list_projects` (or `list_workspaces` / `list_cloud_organizations` to go up a level).
2. `get_project_overview` for the KPI snapshot — customer count, event count, active campaigns
   by channel.
3. Reuse that `project_id` for the rest of the session.

`list_projects_with_overview` fans out one API call **per project** and is slow on large
organizations. Do not call it as a warm-up.

## Reading

- Customers: `list_customers` (paginate with `skip` + `count`, and `skip + count` must not
  exceed 1,000), then `get_customer_properties`, `list_customer_events`,
  `get_customer_attribute_values`.
- Campaigns and flows: `search_scenarios`, `search_email_campaigns`, `search_sms_campaigns`.
  Large design blobs are stripped by default — opt in with `include_design` /
  `include_node_designs` only when you genuinely need the template HTML, because a single
  node's design payload can exceed 70 KB.
- Analytics: `estimate_eql_cost` **before** `execute_analytics_eql`. Ad-hoc analytics on large
  datasets takes 10–30 seconds; that is expected, not a hang.

## Writing — slow down

Write tools change live configuration exactly as a dashboard edit would. There is no automatic
rollback and activity logging is limited. Confirm the target object and the project before
every write.

Rate limits stack, and all applicable limits must pass:

| Class | Limit |
|---|---|
| Global (all tools) | 1 call/second, 60 calls/minute |
| Write tools | 1 call/5 seconds, 10 calls/minute |
| Delete tools | 1 call/10 seconds, 10 calls/minute |

On exhaustion the tool returns the text
`Too many requests: rate limit reached for key '<key>' (<limit>). Retry after ~<N> second(s).`
Wait the stated interval, then retry with exponential backoff. Downstream project APIs impose
their own limits on top, so handle throttling even after an MCP-level check passes.

To author a scenario, read a working one first: call `search_scenarios` with a `scenario_id` and
copy the shape of its `connections` array before calling `create_scenario` or `update_scenario`.
Audience scoping lives on a Condition node inside the graph, **not** on the scenario's
`customer_filter`, which is read-only and effectively always null.

## Caching

Reads may be served from cache — volatile resources for a few seconds, stable metadata for
longer. If you write and then immediately read back, expect a short lag before the change is
visible.
