---
name: Analyze production performance with Polar Signals
description: >-
  Use the Polar Signals MCP server to find a project, discover its profile
  types and labels, and pull a CPU/memory/GPU profile with a PromQL-style
  selector to locate performance regressions and hot functions.
api: mcp/polar-signals-mcp.yml
surface: mcp
operations: [get_projects, profile_types, labels, values, get_profile]
---

# Analyze production performance with Polar Signals

Connect the Polar Signals MCP server first: in the Polar Signals Cloud
dashboard open the **AI** section and follow the instructions to connect your
MCP client and authenticate. All tools operate against the projects your
session can access.

## Steps

1. **List accessible projects** — call `get_projects` and pick the project that
   matches the service you are investigating.
2. **Discover profile types** — call `profile_types` for that project to see
   what is available (e.g. `cpu`, `memory`/heap, GPU profiles).
3. **Explore labels** — call `labels` to list metadata label keys (service,
   pod, node, version, etc.).
4. **Resolve label values** — call `values` for the label key you want to
   filter on to get the concrete values (e.g. a specific `service_version`).
5. **Pull the profile** — call `get_profile` with a PromQL-style selector, for
   example `cpu{service="checkout", namespace="prod"}`, over the time window of
   interest. Compare two versions to surface a regression tied to specific
   functions / code lines.

## Conventions

- Queries use Prometheus-style label selectors (see
  `conventions/polar-signals-conventions.yml`).
- Data is isolated per project — you cannot query across two projects.
- Profiling data contains function memory offsets, not payloads/secrets.
