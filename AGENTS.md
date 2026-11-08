# Amazon Bedrock MCP setup and connection checker

Start with capabilities.json and .agents/skills/pipmcp-amazon-aws-bedrock/SKILL.md. This package uses the private PipMCP gateway. Live discovery/execution requires PIPMCP_API_KEY; no provider runtime or credential mapper is bundled. Actual hosted schemas, selected connections and permissions are authoritative. Run npm test; default npm start is offline. Never claim fixture tests prove live provider access.
