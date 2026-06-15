# Atlas Indexing API

Collection root: `atlas-indexing-service-api`

This collection targets the Atlas `/indexing` module.

## Included Requests

1. `01-index-s3-example.bru` - `POST /indexing/tenants/{{tenantId}}/ingestions`

## Variables

Environment file: `environments/Local.bru`

- `baseUrl` - Atlas base URL
- `tenantId` - tenant id in route path
- `token` - Atlas context JWT
- `jobId` - populated by create request

Collection defaults:

- `baseUrl` defaults to `http://localhost:8080` in `collection.bru`.
- Environment `baseUrl` (if selected) overrides the collection default.

## Notes

- The path tenant must match the JWT tenant claim, or the JWT must contain `tenants: ["*"]`.
- The path tenant still must be onboarded.
- The create request body is intentionally empty; the indexing worker discovers queued tenant data from the Atlas ingestion pipeline.
