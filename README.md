# Amazon Bedrock MCP setup & connection checker

Prepare Amazon Bedrock for an AI workflow: understand the required credentials, generate MCP client configuration and inspect a hosted endpoint’s available tools.

**[Read the managed API import guide](https://pipmcp.com/docs/import-openapi?utm_source=github&utm_medium=readme&utm_campaign=github-amazon-aws-bedrock-mcp&utm_content=readme)** · [Connection guide](https://jannikco.github.io/pipmcp-connectors/integrations/amazon-aws-bedrock/) · [Quick start](#quick-start)

> **Included:** provider setup inventory, MCP client configuration and hosted tool discovery. Provider API execution takes place through the connected service, not this utility.

## A clear path from account to AI client

Use this project when you need to connect an authorized Amazon Bedrock account and want to check the setup before using it in an MCP client. It keeps the provider’s credential prerequisites and the client’s hosted endpoint configuration in one inspectable place.

PipMCP provides a managed path for API import, credential configuration and MCP access. Use its hosted onboarding to connect the provider, then bring the resulting endpoint into your AI client.

No ready-made managed preset is asserted by this repository. Start with the import guide, obtain a compatible API definition and confirm the provider’s authentication requirements before setting up a hosted connection.

## Quick start

Requires **Node.js 22 or later** and npm. The first commands run offline and need no account or credentials.

```bash
git clone https://github.com/jannikco/amazon-aws-bedrock-mcp.git
cd amazon-aws-bedrock-mcp
npm ci --ignore-scripts
npm test
npm start -- --setup
```

The setup command prints this app’s recorded authentication field names, provider documentation, setup link and required client environment variables. It never asks for provider secret values. The output includes:

```json
{
  "app": "Amazon Bedrock",
  "appSlug": "amazon-aws-bedrock",
  "requiredEnvironment": [
    "PIPMCP_ENDPOINT",
    "PIPMCP_TOKEN"
  ]
}
```

## Connect a hosted endpoint

1. [Read the managed API import guide](https://pipmcp.com/docs/import-openapi?utm_source=github&utm_medium=readme&utm_campaign=github-amazon-aws-bedrock-mcp&utm_content=readme) and complete the provider connection or manual import described there.
2. Copy your hosted MCP endpoint and its access token from the product. A provider API key and an MCP endpoint token serve different purposes.
3. In Bash, read the values interactively so the token is not embedded in a command or committed file:

```bash
read -r -p "Hosted MCP endpoint: " PIPMCP_ENDPOINT
read -r -s -p "Hosted MCP token: " PIPMCP_TOKEN
printf '\n'
export PIPMCP_ENDPOINT PIPMCP_TOKEN
npm start
npm start -- --live
```

`npm start` prints MCP client configuration when an endpoint is set; without one it prints the setup inventory. `--live` performs protocol initialization and read-only tool discovery. Its JSON reports `state`, `toolCount`, `toolNames` and `providerLiveValidation: "not-run"`. Use the emitted configuration in your MCP client, then test your intended provider operation there.

## Capabilities and evidence

| Capability | What the released code does | Evidence boundary |
| --- | --- | --- |
| Provider setup inventory | Displays Amazon Bedrock prerequisites and source links | Documented provider metadata; credentials are not validated locally |
| MCP client configuration | Builds configuration for the supplied hosted endpoint | Offline configuration tests |
| Hosted endpoint discovery | Initializes MCP and lists available tools with `--live` | Protocol access only; no `tools/call` |
| Amazon Bedrock API execution | Not implemented in this setup utility | No provider operation or Live outcome is claimed |
| Managed provider operations | Depend on the imported/listed integration, account and permissions | Documented; verify each required operation in the hosted account |

## Amazon Bedrock authentication prerequisites

- Recorded authentication type: **bearer**.
- Provider credential field names: `token`.
- Tenant-specific connection information is required. Use only an account you are authorized to access.

Enter provider credential values only in the intended hosted credential flow. This utility reads the hosted endpoint/token; it does not read or upload the provider’s secret fields. Use the provider’s official guide to confirm scopes, account tier and current prerequisites.

- [Provider API documentation](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_Operations_Amazon_Bedrock_Runtime.html)
- [Authentication / source reference 2](https://docs.aws.amazon.com/bedrock/latest/userguide/api-keys-generate.html)

[Visit Amazon Bedrock](https://aws.amazon.com/bedrock/)

## Frequently asked questions

**Does installing this package let an AI call Amazon Bedrock immediately?**
Complete the hosted connection and configure its MCP endpoint first; this repository handles setup and discovery.

**What should I check if discovery fails?**
Confirm that the endpoint uses HTTPS, the MCP token is current and the hosted connection is enabled. Include sanitized command output when requesting support.

**Where do my credentials go?**
The local utility reads the endpoint and MCP token from the environment. Provider credentials belong in the hosted connection flow. Do not paste either type into issues, source code or an API definition.

**What if a provider has no managed preset?**
Use the manual import guide and a supported API definition. Authentication and required operations must be reviewed before use; a preset is not implied by this repository’s name.

## Related projects

- [Typed LeadAdapter API client](https://github.com/jannikco/linkedin-typescript-sdk) — JavaScript runtime and generated TypeScript request types for the LeadAdapter REST API.
- [LeadAdapter Python API client](https://github.com/jannikco/linkedin-python-sdk) — Standard-library Python client for the LeadAdapter REST API.
- [LinkedIn API quickstart](https://github.com/jannikco/linkedin-api) — Inspect connected profile status and read stored leads through LeadAdapter.
- [LinkedIn people and post search](https://github.com/jannikco/linkedin-search-api) — Build asynchronous people and post search requests with explicit idempotency.

## Support and maintenance

[Report a reproducible issue](https://github.com/jannikco/amazon-aws-bedrock-mcp/issues) with command versions and sanitized output. For sensitive issues, use [PipMCP support](https://pipmcp.com/contact). Never publish tokens, connection URLs containing credentials or customer records.

Maintained by jannikco. MIT applies to this repository’s code; provider services and runtime dependencies have their own terms. Provider names do not imply endorsement. Recorded source revision: `9c557eef3ef93d78aa7eb198fbb2eeb27a4e47cadcb596df93ee0a98d1bf0a8c`. See [integration.json](./integration.json), [LICENSE](./LICENSE) and [SECURITY.md](./SECURITY.md).
