# Security

## Data Flow

InsideOut connects to a hosted MCP server at `https://app.luthersystems.com/v1/insideout-mcp`. Here is what data flows where and why.

### What is sent to the server

Riley is a cloud infrastructure advisor. To recommend the right architecture, it needs to know general tech stack details -- the same information you'd share in the first few minutes of a conversation with a solutions architect. No credentials, secrets, source code, or PII are ever sent.

| Data | When | What it contains | Why Riley needs it |
|------|------|------------------|-------------------|
| **Project context** (anonymized) | Session start (`convoopen`) | General tech stack summary: language, framework, cloud provider, container/K8s usage, CI/CD platform | Determines compute type, avoids conflicting with existing infrastructure, integrates with existing CI/CD |
| **User messages** | Each conversation turn (`convoreply`) | The user's own words, forwarded as-is | The user is talking to Riley -- messages drive the design conversation |
| **Source identifier** | Session start | IDE/tool name (e.g., "codex", "cursor") | Controls the credential connect screen UI so the "Open {IDE}" button works |

### What is NOT sent

- **Credentials** -- AWS/GCP credentials are never sent through the skill or MCP server. Cloud authentication uses browser-based OAuth handled directly by the cloud provider.
- **Source code** -- The skill does not read or transmit source code files.
- **Secrets / API keys** -- The skill never reads `.env`, credential files, or secret stores. The workspace scan only extracts general metadata (language, framework, dependencies).
- **PII** -- No personally identifiable information is collected or transmitted.

### Workspace scanning

Before starting a session, the skill may scan workspace files to build a project context string. This exists so Riley can make informed architecture decisions (e.g., recommending ECS for a containerized Node.js app, or Cloud Run for a Go service already on GCP) without asking the user a dozen discovery questions.

1. **Requires user consent** -- The agent must ask the user before scanning and show them what will be sent.
2. **Extracts only general metadata** -- Language/framework (from `package.json`, `go.mod`, etc.), cloud provider hints, container usage, CI/CD platform.
3. **Excludes sensitive files** -- Never reads `.env`, credentials, secrets, private keys, or files containing PII.
4. **Produces a short summary** -- The context is a few lines of plain text like "Node.js 20, TypeScript, Next.js 15, AWS, GitHub Actions." No file contents are sent.
5. **Optional** -- If the user declines, Riley asks discovery questions instead. The scan is a convenience, not a requirement.

### Cloud credentials

When deploying infrastructure, InsideOut uses browser-based authentication:

1. Riley provides a URL for the user to open in their browser.
2. The user authenticates directly with AWS or GCP through their browser.
3. Credentials are managed by the InsideOut platform -- they are never passed through the agent or stored by the skill.

The `credawait` tool polls for credential status after the user completes browser authentication. It does not transmit or receive credentials.

## Security Audit Notes

Automated security scanners (Snyk, Socket) may flag this skill because it:

- Connects to a remote MCP server
- Scans workspace files and sends context to that server
- Can trigger real-world cloud actions (deploy, destroy)

These are fundamental to how InsideOut works -- it is a remote infrastructure management tool. The same pattern applies to all remote MCP skills (Linear, Figma, Sentry, etc.). The flags reflect the trust model of remote MCP skills generally, not a vulnerability specific to InsideOut.

## Reporting Vulnerabilities

To report a security issue, email security@luthersystems.com or use the `submit_feedback` MCP tool with category `"security"`.
