# Amazon Bedrock MCP setup and connection checker

**Prepare Amazon Bedrock for AI workflows with a PipMCP setup doctor, agent instructions and authenticated connector discovery.**

[Activate with PipMCP](https://pipmcp.com/github/start?integration=openapi-to-mcp&utm_source=github&utm_campaign=github-amazon-aws-bedrock-mcp) · [Integration directory](https://jannikco.github.io/pipmcp-connectors/integrations/amazon-aws-bedrock/)

## Build something useful

Prepare Amazon Bedrock and inspect your hosted setup

This repository is a setup utility. Availability depends on your actual hosted integration and connection.

## Quick start

Requires Node.js 22+. Clone this repository, then:

```sh
npm ci --ignore-scripts
npm test
npm start
# After configuring your provider in PipMCP and setting PIPMCP_API_KEY:
npm start -- --live
# Launch the stdio MCP bridge:
npm run server
```

Create a PipMCP API key with explicit connectors:read and connectors:call scopes. Set PIPMCP_API_KEY in the calling process environment. Provider credentials are configured in PipMCP. Multiple accounts require explicit PIPMCP_INTEGRATION_ID and PIPMCP_CONNECTION_ID from discovery. Existing keys do not gain scopes automatically.

## Built for coding agents

Install this repository's skill into your project:

```sh
node lib/cli.mjs agents install --client codex --scope project --project /path/to/project
# Use claude or grok for those clients.
```

Connect AI agents to Amazon Bedrock using one PipMCP API key and actual hosted tool discovery. Native AGENTS.md, CLAUDE.md and project skills support the current coding session. No additional AI model key is required. [capabilities.json](capabilities.json) describes the package; [llms.txt](llms.txt) indexes its instructions.

## Inspect actual tools before calling

```js
import {PipMCPGatewayClient} from './lib/index.mjs';
const client = new PipMCPGatewayClient(); // PIPMCP_API_KEY
const inventory = await client.getTools("amazon-aws-bedrock");
console.log(inventory.data.tools);
// Select a real tool and validate your arguments against its returned schema.
// client.callTool(provider, actualToolName, reviewedArguments, {idempotencyKey});
```

PipMCP enforces workspace, role, connection, policy/approval and usage controls. Preserve the original idempotency key when retrieving/resuming a receipt. Pending and confirmation_required are not completed provider results; uncertain writes are held for reconciliation. The bridge never auto-approves a hosted action.

## Version 1 migration

This release uses the authenticated PipMCP gateway for real discovery/execution. Upgrade your MCP launch command and configure PIPMCP_API_KEY before reconnecting. Historical local runtimes remain available under their original MIT license in Git history. 

## Evidence and support

Transport, package and agent fixtures are tested. Authentication/provider access must be checked in your own configured workspace; synthetic fixtures do not establish live provider execution. Do not include credentials or customer records in public issues. MIT covers this public client.

## Use a manually imported API

If the provider is not already available in your workspace, import your reviewed API definition, publish the selected tools and configure its connection in PipMCP. Set PIPMCP_CONNECTOR_ID=custom-api together with PIPMCP_INTEGRATION_ID and, when needed, PIPMCP_CONNECTION_ID. Then run npm run doctor or npm run server. These select your published private API; they do not change the fixed PipMCP origin or bypass hosted authorization. Leaving PIPMCP_CONNECTOR_ID unset uses amazon-aws-bedrock. Importing requires integrations:read and integrations:write on the same PipMCP key; discovery and calls use connectors:read and connectors:call.

## Provider setup reference

[Official provider documentation](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_Operations_Amazon_Bedrock_Runtime.html). Credentials are entered in PipMCP; this package does not accept or store provider secrets.
