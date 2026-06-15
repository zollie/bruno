# Atlas Flows API (Bruno Collection)

Collection root: `docs/bruno/atlas-flows-api`

## Included Requests

1. `01-health.bru` - `GET /flows/health`
2. `02-create-flow.bru` - `POST /flows/tenants/{{tenantId}}/flows` (source-type gdrive create)
3. `03-list-flows.bru` - `GET /flows/tenants/{{tenantId}}/flows?limit=20`
4. `04-get-flow-by-id.bru` - `GET /flows/tenants/{{tenantId}}/flows/{{flowId}}`
5. `05-trigger-flow.bru` - `POST /flows/tenants/{{tenantId}}/flows/{{flowId}}:trigger`
6. `06-execute-flow.bru` - `POST /flows/tenants/{{tenantId}}/flows/{{flowId}}/executions`
7. `07-delete-flow.bru` - `DELETE /flows/tenants/{{tenantId}}/flows/{{flowId}}`
8. `08-create-flow-all-files-2-top-level-tasks.bru` - `POST /flows/tenants/{{tenantId}}/flows` (source-type gdrive create)
9. `09-list-executions.bru` - `GET /flows/tenants/{{tenantId}}/flows/{{flowId}}/executions?limit=100`
10. `Create Flow (no Id).bru` - `POST /flows/tenants/{{tenantId}}/flows` (gdrive source-type payload example)
11. `13-upload-to-s3.bru` - `POST /flows/tenants/{{tenantId}}/uploads` (context-catalog entry upload to managed S3)
12. `14-delete-all-flows.bru` - `DELETE /flows/tenants/{{tenantId}}/flows`
13. `15-upload-file.bru` - `POST /flows/tenants/{{tenantId}}/uploads` (multipart file upload)

## Variables

Environment file: `environments/Local.bru`

- `baseUrl` - Atlas base URL (default `http://localhost:8080`)
- `tenantId` - tenant path parameter for all data routes
- `token` - bearer JWT (must include `tenant_id` and `vertical`; `tenant_id` must match `tenantId`)
- `flowId` - populated after create request
- `executionId` - populated after trigger/execute request
- `uploadFilePath` - local file path used by upload requests

## Source Format

Create requests use the source-type contract:

```json
{
  "sourceType": "gdrive",
  "source": {
    "folderId": "<gdrive-folder-id>",
    "serviceAccount": { "...": "..." }
  }
}
```

Legacy payloads (`source` + `sinks`) are no longer accepted.

## Direct Upload Route

`POST /flows/tenants/{{tenantId}}/uploads` accepts `multipart/form-data` with a required `file` part and returns:

- `tenantId`
- `bucket`
- `key`
- `s3Uri`
- `etag`
- `sizeBytes`
- `uploadedAt`

For context-catalog upload repros, set `tenantId` and `token` to the same tenant. The token must include both `tenant_id` and `vertical`; the server rejects malformed multipart payloads with `400` and tenant/token mismatches with `403`.

## Import into Bruno

Import the `atlas-flows-api` folder as a collection in Bruno.
Bruno will read `bruno.json` + `.bru` files directly.
