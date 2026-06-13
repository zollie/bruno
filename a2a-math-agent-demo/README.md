# A2A Math Agent Demo Collection

This demo creates, lists, and invokes a deterministic math agent through the Atlas coordinator.

## Flow

1. `01-agents-health`
2. `02-create-math-agent`
3. `03-list-agents`
4. `04-invoke-through-coordinator`

## Variables

- `baseUrl` - Atlas Backend base URL (default `http://localhost:8080`)
- `tenantId` - active tenant for the request path
- `token` - bearer JWT used for Atlas requests

## Atlas auth behavior

- Requests use `Authorization: Bearer {{token}}`.
- The active tenant is explicit in the request path.
- `02-create-math-agent` calls `POST /agents/tenants/{tenantId}`.
- `04-invoke-through-coordinator` calls `POST /tenants/{tenantId}/a2a/invoke` through Atlas.

## Agent create payload

`02-create-math-agent.bru` creates an Atlas agent with:

- `name: Math Agent`
- `description: Deterministic math solver with strict structured output`
- `prompt: Solve numeric and symbolic math tasks and return strict JSON.`
- `tools: []`
- deterministic math skill `math_solve`

## Coordinator invoke

`04-invoke-through-coordinator.bru` sends an A2A `message/send` request to the tenant coordinator for the configured `tenantId`.
