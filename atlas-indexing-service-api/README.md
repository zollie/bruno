# Atlas Indexing Service API

Collection root: `atlas-indexing-service-api`

This collection targets the `/v1/index` S3 indexing route.

## Included Requests

1. `01-index-s3-example.bru` - `POST /v1/index` with S3 input

## Variables

Environment file: `environments/Local.bru`

- `baseUrl` - indexing service base URL
- `token` - bearer token sent in the `Authorization` header
- `tenantId` - tenant id sent in request body as `tenant_id`
- `s3Bucket` - source S3 bucket
- `s3Key` - source S3 object key
- `sourceFilename` - source file name reported to indexing
- `contentType` - source file content type

Collection defaults:

- `baseUrl` defaults to `http://localhost:8081` in `collection.bru`.
- Environment `baseUrl` (if selected) overrides the collection default.

## Notes

- Payload includes `tenant_id` in body; this is not path-scoped like Atlas module routes.
- Send the token as `Authorization: Bearer {{token}}`.
- `input.type` supports `s3` and `text`; this collection currently includes the S3 example.
