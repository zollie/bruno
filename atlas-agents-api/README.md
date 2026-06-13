# Atlas Agents API (Bruno Collection)

Collection root: `docs/bruno/atlas-agents-api`

## Included Requests

1. `01-health.bru` - `GET /agents/health`
2. `02-create-agent.bru` - `POST /agents/tenants/{{tenantId}}`
3. `03-list-agents.bru` - `GET /agents/tenants/{{tenantId}}`
4. `04-get-agent.bru` - `GET /agents/tenants/{{tenantId}}/{{agentId}}`
5. `05-update-agent.bru` - `PUT /agents/tenants/{{tenantId}}/{{agentId}}`
6. `06-delete-agent.bru` - `DELETE /agents/tenants/{{tenantId}}/{{agentId}}`
7. `07-onboard-tenant-runtime.bru` - `POST /agents/tenants/{{tenantId}}/runtime`
8. `08-get-coordinator-card.bru` - `GET /tenants/{{tenantId}}/a2a/.well-known/agent-card.json`
9. `09-delete-tenant-runtime.bru` - `DELETE /agents/tenants/{{tenantId}}/runtime`
10. `10-get-peer-agent-card.bru` - `GET /tenants/{{tenantId}}/a2a/agents/{{agentId}}/.well-known/agent-card.json`
11. `11-invoke-peer-agent.bru` - `POST /tenants/{{tenantId}}/a2a/agents/{{agentId}}/invoke`

## Variables

Environment file: `environments/Local.bru`

- `baseUrl` - Atlas base URL (default `http://localhost:8080`)
- `token` - bearer JWT used for all `/agents` data routes
- `agentId` - populated by create request
- `sourceAgentId` - source/calling agent id for peer A2A calls
- `sourceAgentToken` - projected ServiceAccount token for `sourceAgentId`
- `tenantId` - active tenant for the request path

## Auth Notes

- Data routes include `Authorization: Bearer {{token}}`.
- Public coordinator A2A routes use the user `Authorization` header.
- Peer A2A routes also require `X-Atlas-A2A-Source-Agent-ID` and `X-Atlas-A2A-Agent-Authorization`.
- Health route does not require auth in this collection.

## Suggested Run Order

1. `01-health.bru`
2. `02-create-agent.bru`
3. `03-list-agents.bru`
4. `04-get-agent.bru`
5. `05-update-agent.bru`
6. `04-get-agent.bru` (re-run to verify updated values)
7. `07-onboard-tenant-runtime.bru`
8. `08-get-coordinator-card.bru`
9. `10-get-peer-agent-card.bru` (requires source-agent headers)
10. `11-invoke-peer-agent.bru` (requires source-agent headers)
11. `06-delete-agent.bru`
12. `04-get-agent.bru` (optional, expect `404` after delete)

## Import into Bruno

Import the `atlas-agents-api` folder as a collection in Bruno.
Bruno will read `bruno.json` + `.bru` files directly.
