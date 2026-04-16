# Agent Skills by Luther Systems

## Portable AI Skills

This repo aggregates Luther Systems agent skills. Each skill lives in its own repo and is included here as a git submodule.

| Skill | Repo | Install |
|-------|------|---------|
| **InsideOut** | [luthersystems/insideout-agent-skills](https://github.com/luthersystems/insideout-agent-skills) | `npx skills add luthersystems/insideout-agent-skills` |
| **Substrate** | [luthersystems/substrate-agent-skills](https://github.com/luthersystems/substrate-agent-skills) | `npx skills add luthersystems/substrate-agent-skills` |

You can also install skills directly from this repo using the `--path` flag:

```bash
npx skills add luthersystems/agent-skills --path insideout
npx skills add luthersystems/agent-skills --path substrate
```

Works with **Claude Code**, **Codex**, **Cursor**, **Kiro**, **Windsurf**, and [30+ other tools](https://agentskills.io).

**[Try the web app](https://insideout.luthersystemsapp.com/)** -- no install required.

---

## What You Can Do

- Design AWS/GCP architectures conversationally
- Get real-time cost estimates before deploying
- Generate production-ready Terraform
- Deploy infrastructure directly from your tool
- Inspect environments and detect drift
- Monitor logs and manage resources over time

---

## Who Is This For?

This is for developers, platform engineers, and AI-native builders who want to manage infrastructure without switching tools or writing everything manually.

- **Developers** who want to go from idea to deployed backend quickly
- **DevOps / platform engineers** who want faster architecture and IaC workflows
- **Builders using AI coding tools** who want infrastructure built into their workflow

If you'd rather describe what you want than manually wire it together -- this is for you.

---

## Quick Start

Once installed, just ask:

> "Build me a scalable backend for my golang microservices app on AWS with a DB and auth."

InsideOut will:
1. Propose an architecture (ECS, RDS, Cognito, ALB, VPC...)
2. Estimate monthly cost
3. Generate production-ready Terraform
4. Deploy to your AWS or GCP account

```
You: "I need infrastructure for an e-commerce platform, 1k monthly users on AWS"

Riley: "I'd recommend ECS for your containers, RDS PostgreSQL for your database,
        ElastiCache Redis for sessions, and an ALB. Estimated cost: ~$350/month.
        Want me to adjust anything?"

You: "Looks good, generate the Terraform"

[Generates production-ready Terraform files]

You: "Deploy it"

[Deploys to AWS, streams logs in real-time]
```

---

## Choose Your Path

There are a few ways to use InsideOut depending on your setup:

| Path | Best for | Install |
|------|----------|---------|
| **[Standalone repo](https://github.com/luthersystems/insideout-agent-skills)** (recommended) | Portable skill across multiple AI tools | `npx skills add luthersystems/insideout-agent-skills` |
| **[Claude Code plugin](https://github.com/luthersystems/insideout-claude-code)** | Best native Claude Code experience | `/plugin marketplace add luthersystems/insideout-claude-code` |
| **[Kiro power](https://github.com/luthersystems/insideout-power)** | Best native Kiro experience | Add power from GitHub: `luthersystems/insideout-power` |
| **[Web app](https://insideout.luthersystemsapp.com/)** | No install required | Open in browser |

---

## Installation (Detailed)

### Recommended: Skills CLI

```bash
npx skills add luthersystems/insideout-agent-skills
```

> **Note:** You can also install from this aggregator repo: `npx skills add luthersystems/agent-skills --path insideout`

### Claude Code

For the full plugin experience (slash commands, agents, hooks, auto-approve):

```
/plugin marketplace add luthersystems/insideout-claude-code
/plugin install insideout
```

### Codex

```bash
npx skills add luthersystems/insideout-agent-skills --agent codex
```

Activate by typing `$insideout` -- Codex will detect the missing MCP server and prompt you to install it automatically.

> **Fallback:** If the auto-install doesn't trigger, run: `codex mcp add insideout --url https://app.luthersystems.com/v1/insideout-mcp`

### Cursor

```bash
npx skills add luthersystems/insideout-agent-skills --agent cursor
```

Then add the MCP server to your Cursor MCP config:

```json
{
  "mcpServers": {
    "insideout": {
      "type": "http",
      "url": "https://app.luthersystems.com/v1/insideout-mcp"
    }
  }
}
```

### Windsurf

```bash
npx skills add luthersystems/insideout-agent-skills --agent windsurf
```

Add MCP via Windsurf MCP Marketplace or manually in settings.

### Kiro

For the full power experience (steering files, design pattern guides, auto-approve):

1. Open the Powers panel
2. Click **Add power from GitHub**
3. Enter: `luthersystems/insideout-power`

### Antigravity

```bash
npx skills add luthersystems/insideout-agent-skills --agent antigravity
```

Then add the MCP server in Antigravity: "..." menu → MCP Servers → Manage MCP Servers → View raw config:

```json
{
  "mcpServers": {
    "insideout": {
      "serverUrl": "https://app.luthersystems.com/v1/insideout-mcp"
    }
  }
}
```

> **Note:** Antigravity uses `serverUrl`, not `url`.

### Manual Installation

```bash
git clone https://github.com/luthersystems/insideout-agent-skills.git
cp -r insideout-agent-skills ~/.agents/skills/insideout
```

The `~/.agents/skills/` directory is the standard cross-tool location. Most AI coding tools will detect skills here automatically.

---

## Supported Services (60+)

| Category | AWS | GCP |
|----------|-----|-----|
| **Compute** | EC2, ECS, EKS, Lambda | Compute Engine, Cloud Run, GKE, Cloud Functions |
| **Database** | RDS PostgreSQL, DynamoDB, ElastiCache, OpenSearch | Cloud SQL, Firestore, Memorystore |
| **Networking** | VPC, ALB, CloudFront, API Gateway | VPC, Load Balancing, Cloud CDN, API Gateway |
| **Storage** | S3 | Cloud Storage |
| **Security** | WAF, KMS, Secrets Manager, Cognito | Cloud Armor, Cloud KMS, Secret Manager, Identity Platform |
| **Messaging** | SQS, MSK (Kafka) | Pub/Sub |
| **Observability** | CloudWatch, Managed Grafana | Cloud Logging, Cloud Monitoring |
| **AI/ML** | Bedrock | Vertex AI |
| **CI/CD** | CodePipeline, GitHub Actions | Cloud Build |
| **Backup** | AWS Backup | GCP Backups |

---

## Security & Auth Model

- **No API key required** to install or use the skill
- The MCP server runs remotely over HTTPS -- no local binary needed
- Cloud credentials are only required at deployment time, via a browser-based connect flow
- You control what gets deployed into your cloud accounts
- You can review all generated Terraform before deploying

---

## How It Works

InsideOut uses a multi-agent AI system behind a single MCP server:

| Agent | Role |
|-------|------|
| **Riley** | Infrastructure advisor -- leads the design conversation |
| **Hippo** | Cost estimation and pricing optimization |
| **Joy** | User experience and requirement gathering |
| **Etch** | Terraform code generation |
| **Core** | Architecture validation and best practices |
| **Axel** | Deployment orchestration |

From your perspective, you're talking to Riley. The other agents work behind the scenes. These capabilities are exposed as tools inside your AI environment, so you use them conversationally.

---

## Resources

- [InsideOut Platform](https://insideout.luthersystems.com)
- [Web App](https://insideout.luthersystemsapp.com/) -- try InsideOut without any IDE
- [Demo Video](https://insideout.luthersystems.com/demo-video)
- [Terraform Presets](https://github.com/luthersystems/insideout-terraform-presets) -- the pre-built modules InsideOut composes
- [Agent Skills Standard](https://agentskills.io)
- [Subreddit](https://www.reddit.com/r/luthersystems/)

---

## About Luther Systems

Our mission is to accelerate the advent of the automated enterprise. Founded by a team who spent years automating complex processes for some of the world's largest financial institutions, Luther Systems builds platforms that bring enterprise-grade infrastructure and operations to teams of every size.

- [Luther Systems](https://luthersystems.com)
- [InsideOut](https://insideout.luthersystems.com)

---

## Get Help

- [Discord](https://insideout.luthersystems.com/discord) -- chat with the devs and InsideOut users
- [Book a Call](https://insideout.luthersystems.com/general-call) -- talk with us
- [Tech Call](https://insideout.luthersystems.com/tech-call) -- talk with the devs
- Email: [contact@luthersystems.com](mailto:contact@luthersystems.com)

---

## License

[Apache License 2.0](LICENSE)
