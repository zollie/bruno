# Atlas Indexing Service API (Standalone Only)

Collection root: `atlas-indexing-service-api`

This collection is only for the standalone indexing service that exposes `/v1/index`.
For Atlas `api-blue`, use the sibling `atlas-indexing-api` collection.

## Included Requests

1. `01-index-s3-example.bru` - `POST /v1/index` with S3 input

## Variables

Environment file: `environments/Local.bru`

- `baseUrl` - standalone indexing service base URL
- `apiKey` - optional service API key used as `X-API-Key` when `INDEXING_SERVICE__API_KEY` is enabled

Collection defaults:

- `baseUrl` defaults to `http://localhost:8081` in `collection.bru`.
- Environment `baseUrl` (if selected) overrides the collection default.

## Notes

- Payload includes `tenant_id` in body; this is not path-scoped like Atlas module routes.
- This service does not use JWT bearer auth.
- Do not point this collection at `api-blue`; that host uses Atlas module routes under `/indexing`.
- If API key auth is enabled, set `X-API-Key` header with `{{apiKey}}`.
- `input.type` supports `s3` and `text`; this collection currently includes the S3 example.
