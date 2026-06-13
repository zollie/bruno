# Atlas Gateway API (Bruno Collection)

Collection root: `atlas-gateway-api`

This collection targets the Atlas Gateway module. Customer-facing Gateway APIs are tenant-scoped under `/gateway/tenants/{{tenantId}}`.

## Included Requests

1. `01-health.bru` - `GET /gateway/health`
2. `02-healthz.bru` - `GET /gateway/healthz`
3. `03-metrics.bru` - `GET /gateway/metrics`
4. `10-runtime-mcp-tools-list.bru` - `POST /gateway/tenants/{{tenantId}}/mcp` (`tools/list`)
5. `11-runtime-mcp-tools-call.bru` - `POST /gateway/tenants/{{tenantId}}/mcp` (`tools/call`)
6. `20-tools-list.bru` - `GET /gateway/tenants/{{tenantId}}/tools`
7. `21-tool-create.bru` - `POST /gateway/tenants/{{tenantId}}/tools`
8. `22-tool-get.bru` - `GET /gateway/tenants/{{tenantId}}/tools/{{toolId}}`
9. `23-tool-update.bru` - `PUT /gateway/tenants/{{tenantId}}/tools/{{toolId}}`
10. `24-tool-delete.bru` - `DELETE /gateway/tenants/{{tenantId}}/tools/{{toolId}}`
11. `30`-`35` - tenant admin targets
12. `40`-`44` - tenant admin bindings
13. `50`-`54` - tenant admin OpenAPI import/specs/operations
14. `61-kb-tool-create-query.bru` - `POST /gateway/tenants/{{tenantId}}/tools` (`knowledge_query`)
15. `62-kb-tool-create-memory.bru` - `POST /gateway/tenants/{{tenantId}}/tools` (`knowledge_memory`)
16. `66-kb-verify-tools.bru` - `GET /gateway/tenants/{{tenantId}}/tools`
17. `68-kb-verify-query-tool.bru` - `GET /gateway/tenants/{{tenantId}}/tools/{{kbQueryToolId}}`
18. `69-kb-verify-memory-tool.bru` - `GET /gateway/tenants/{{tenantId}}/tools/{{kbMemoryToolId}}`
19. `70-kb-runtime-mcp-call-query.bru` - `POST /gateway/tenants/{{tenantId}}/mcp` (`tools/call` -> `knowledge_query`)
20. `71-kb-runtime-mcp-call-memory.bru` - `POST /gateway/tenants/{{tenantId}}/mcp` (`tools/call` -> `knowledge_memory`)
21. `72-kb-runtime-mcp-call-knowledge-base.bru` - `POST /gateway/tenants/{{tenantId}}/mcp` (`tools/call` -> `knowledge_base`)

## Variables

Environment file: `environments/Local.bru`

- `baseUrl` - Atlas base URL (default `http://localhost:8080`)
- `tenantId` - active tenant in the Gateway path
- `userToken` - valid JWT for runtime endpoints
- `adminToken` - valid JWT with admin role (default role name: `gateway_admin`)
- `toolId`, `toolName` - populated by tool CRUD requests
- `targetId`, `targetName`, `targetBaseUrl`, `targetMcpPath` - target admin variables
- `bindingId` - populated by binding create
- `openApiSpecId`, `openApiName`, `operationId` - OpenAPI admin variables
- `kbQueryToolId`, `kbMemoryToolId`, `kbQueryToolName`, `kbMemoryToolName` - populated by KB tool create requests
- `kbKnowledgeToolName` - tenant runtime KnowledgeBase tool created by `POST /agents/tenant`

## Knowledge Base Tools Runbook

Execute the KB tool requests in this order:

1. `61-kb-tool-create-query.bru`
2. `62-kb-tool-create-memory.bru`
3. `66-kb-verify-tools.bru`
4. `68-kb-verify-query-tool.bru`
5. `69-kb-verify-memory-tool.bru`
6. `70-kb-runtime-mcp-call-query.bru`
7. `71-kb-runtime-mcp-call-memory.bru`
8. `72-kb-runtime-mcp-call-knowledge-base.bru` after tenant runtime onboarding

Important: `tenant_id` is required in both `knowledge_query` and `knowledge_memory` tool arguments. Use the same value as `tenantId`.
Important: runtime KB calls require existing gateway bindings for these tool names.
Tool definitions in `61` and `62` are synced to `../knowledge-base-tools/internal/tools/query/query.go` and `../knowledge-base-tools/internal/tools/memory/memory.go`.

## Admin Runbooks

Target discovery requires `targetBaseUrl` and `targetMcpPath` to point at a reachable MCP server. If those values are placeholders, create/get/update/list still exercise the Gateway API, but `34-target-discover.bru` will fail against the downstream target.

For a basic MCP binding smoke test:

1. `21-tool-create.bru`
2. `31-target-create.bru`
3. `41-binding-create.bru`
4. `10-runtime-mcp-tools-list.bru`
5. `11-runtime-mcp-tools-call.bru`

For OpenAPI admin coverage:

1. `50-openapi-import.bru`
2. `51-openapi-specs-list.bru`
3. `52-openapi-spec-get.bru`
4. `53-openapi-operations-list.bru`
5. `54-openapi-spec-delete.bru`

## Auth Notes

- Gateway validates JWT signatures (RS256) via JWKS.
- The active tenant comes from `/gateway/tenants/{{tenantId}}/...`.
- Tokens must include access to the path tenant through `tenant_id`, `tenants`, or `tenant_ids`; wildcard `["*"]` works only for tokens already allowed by Atlas.
- Runtime requests use `userToken`; management requests under `/tools` and `/admin/*` use `adminToken`.
- Management requests require the configured admin role (default: `gateway_admin`).
- `tools/call` arguments are passed into Gateway authz context, so do not strip important tool arguments from examples.

## Sandbox Blue

Use `environments/Sandbox Blue.bru`. Its `baseUrl` is `https://api-blue.us-east-1.sandbox.avidaws.com`.

## Import into Bruno

Import the `atlas-gateway-api` folder as a collection in Bruno.
Bruno will read `bruno.json` + `.bru` files directly.
