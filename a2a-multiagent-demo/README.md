# A2A Multi-Agent Demo Collection

## Scope

This demo collection simulates a deterministic multi-agent setup:

- `Coordinator` agent (delegates and summarizes the workflow)
- `Planner` agent (breaks request into concrete steps)
- `Validator` agent (checks final output and returns risk/completeness status)

All three are expected to:

- expose `/.well-known/agent-card.json`
- expose A2A JSON-RPC at `/invoke`

## Run Steps

1. `01-registry-health`
2. `02-publish-coordinator-agent`
3. `03-publish-planner-agent`
4. `04-publish-validator-agent`
5. `05-list-registry-agents`
6. `06-registry-discovery-index`
7. `07-invoke-coordinator`
8. `08-get-coordinator-task`
9. `09-invoke-planner`
10. `10-get-planner-task`
11. `11-invoke-validator`
12. `12-get-validator-task`

## Variables

- `registryBaseUrl`: A2A registry URL
- `coordinatorAgentBaseUrl`, `plannerAgentBaseUrl`, `validatorAgentBaseUrl`: per-agent base URLs
- `token`: JWT for protected registry endpoints (`/agents`, `/.well-known/agents/index.json`)
- `coordinatorCardUrl` etc.: card URLs used for publish-by-cardUrl (`POST /agents`)
- `workflowRunId`: run marker included in message IDs and JSON-RPC IDs

## Notes

- This repo’s registry router exposes `POST /agents` without auth, so publish requests intentionally do not send auth.
- For stricter auth enforcement environments, add `Authorization` if needed.
- Registry responses for `/agents` are token-based in this codebase.
