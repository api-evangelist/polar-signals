---
name: Provision a Polar Signals project and ingest token
description: >-
  Use the Polar Signals Cloud gRPC/Connect API to create a project under an
  organization and mint a service-account token so an eBPF agent or CI job can
  push profiles.
api: grpc/polar-signals-project.proto
surface: grpc
operations: [GetOrganizations, CreateProject, CreateServiceAccount, CreateServiceAccountToken, CreateProjectToken]
---

# Provision a Polar Signals project and ingest token

Authenticate with a service-account bearer token (`psc_v1_...`) via the
`Authorization: Bearer` header against `api.polarsignals.com` (gRPC/Connect).
See `authentication/polar-signals-authentication.yml`.

## Steps

1. **Find the organization** — call `OrganizationService.GetOrganizations` and
   select the target `organization_id`.
2. **Create the project** — call `ProjectService.CreateProject` with the org id
   and a project name (e.g. "Production"). Projects are the data-isolation
   boundary.
3. **Create a service account** — call
   `ServiceAccountService.CreateServiceAccount` for automation identity.
4. **Mint a token** — call `ServiceAccountService.CreateServiceAccountToken`
   (or `ProjectService.CreateProjectToken`). The `psc_v1_` token is shown
   **once** — capture it immediately.
5. **Configure ingest** — give the agent the token via
   `--remote-store-bearer-token-file` and select the project with the
   `projectID` gRPC metadata header (`--remote-store-grpc-headers`).

## Conventions

- Bearer token in `Authorization`; project selected via `projectID` metadata
  (see `conventions/polar-signals-conventions.yml`).
- RBAC governs what a token can do; check with `AuthorizationService.CanI`.
- Errors surface as gRPC/Connect status codes.
