# Atlas Authz API (Bruno Collection)

Collection root: `docs/bruno/atlas-authz-api`

## Included Requests

1. `01-health.bru` - `GET /authz/health`
2. `02-authorize.bru` - `POST /authz/tenants/{{tenantId}}/authorize`
3. `03-authorize-gateway-tool-create-admin.bru` - `POST /authz/tenants/{{tenantId}}/authorize` (`tool.create` with `isAdmin=true`, expected allow)
4. `04-authorize-gateway-tool-create-non-admin.bru` - `POST /authz/tenants/{{tenantId}}/authorize` (`tool.create` with `isAdmin=false`, expected deny)
5. `05-policies-upsert-tenant.bru` - `PUT /authz/tenants/{{tenantId}}/policies` (create/update tenant policy bundle)
6. `06-policies-get-tenant.bru` - `GET /authz/tenants/{{tenantId}}/policies`
7. `07-policies-list.bru` - `GET /authz/tenants/{{tenantId}}/policies`
8. `08-policies-delete-tenant.bru` - `DELETE /authz/tenants/{{tenantId}}/policies`
9. `09-authorize-agent-invoke.bru` - `POST /authz/tenants/{{tenantId}}/authorize` (`agent.invoke`, expected same-tenant allow)
10. `10-authorize-agent-invoke-cross-tenant-deny.bru` - `POST /authz/tenants/{{tenantId}}/authorize` (`agent.invoke`, expected cross-tenant deny)

## Variables

Environment file: `environments/Local.bru`

- `baseUrl` - Atlas base URL (default `http://localhost:8080`)
- `tenantId` - tenant id used in header/body
- `userId` - principal identity used in authorize request
- `adminUserId` - principal identity used in admin tool-create authz request
- `nonAdminUserId` - principal identity used in non-admin tool-create authz request
- `toolName` - resource tool name used in authorize request
- `agentResourceId` - logical Atlas agent id used in A2A authz checks
- `crossTenantId` - different tenant id used for deny checks
- `gatewayToolsResourceId` - logical resource id for gateway tool CRUD checks
- `token` - bearer token for authz when JWT verification is enabled

## Auth Notes

- With default config (`AUTHZ_REQUIRE_JWT=true`), `token` must be a valid RS256 JWT accepted by JWKS.
- If JWT verification is disabled (`AUTHZ_REQUIRE_JWT=false`), auth header is optional.
- `POST /authz/tenants/{{tenantId}}/authorize` can return `404` if the tenant policy bundle is missing.
- `/authz/tenants/{{tenantId}}/policies` endpoints require `isAdmin=true` in validated JWT claims when JWT auth is enabled.

## Runtime + Gateway Policy Flow

Run this sequence:

1. `05-policies-upsert-tenant.bru`
2. `02-authorize.bru`
3. `09-authorize-agent-invoke.bru`
4. `10-authorize-agent-invoke-cross-tenant-deny.bru`
5. `03-authorize-gateway-tool-create-admin.bru`
6. `04-authorize-gateway-tool-create-non-admin.bru`

Expected:
- request `02` returns `{"allow": true}` for same-tenant `tool.call`
- request `09` returns `{"allow": true}` for same-tenant `agent.invoke`
- request `10` returns `{"allow": false}` for cross-tenant `agent.invoke`
- request `03` returns `{"allow": true}` for admin `tool.create`
- request `04` returns `{"allow": false}` for non-admin `tool.create`

## Import into Bruno

Import the `atlas-authz-api` folder as a collection in Bruno.
Bruno will read `bruno.json` + `.bru` files directly.
