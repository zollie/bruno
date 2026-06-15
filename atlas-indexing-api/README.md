# Atlas Indexing API (Bruno Collection)

Collection root: `docs/bruno/atlas-indexing-api`

## Included Requests

1. `01-health.bru` - `GET /indexing/health`
2. `02-create-ingestion.bru` - `POST /indexing/tenants/{{tenantId}}/ingestions`
3. `03-list-ingestions.bru` - `GET /indexing/tenants/{{tenantId}}/ingestions?limit=20`
4. `04-get-ingestion.bru` - `GET /indexing/tenants/{{tenantId}}/ingestions/{{jobId}}`
5. `05-list-failures.bru` - `GET /indexing/tenants/{{tenantId}}/ingestions/{{jobId}}/failures?limit=20`
6. `06-cancel-ingestion.bru` - `POST /indexing/tenants/{{tenantId}}/ingestions/{{jobId}}:cancel`
7. `07-delete-all-ingestions.bru` - `DELETE /indexing/tenants/{{tenantId}}/ingestions`
8. `08-stop-ingestions.bru` - `POST /indexing/tenants/{{tenantId}}/ingestions/stop`

## Variables

Environment file: `environments/Local.bru`

- `baseUrl` - Atlas base URL (default `http://localhost:8080`)
- `tenantId` - tenant id in route path
- `token` - bearer token containing `tenant_id` claim matching `tenantId`
- `jobId` - populated by create request

## Auth Notes

- Indexing middleware extracts `tenant_id` from JWT payload.
- Path tenant must match JWT tenant, or the JWT must contain `tenants: ["*"]`.
- The path tenant still must be onboarded.

## Import into Bruno

Import the `atlas-indexing-api` folder as a collection in Bruno.
Bruno will read `bruno.json` + `.bru` files directly.
