<p align="center">
  <img src="assets/banner.svg" alt="Luther Systems -- Agent Skills" width="100%">
</p>

<p align="center">
  <strong>Portable agent skills for Luther Systems products -- works with Claude Code, Codex, Cursor, Kiro, Windsurf, and 30+ other tools</strong>
</p>

<p align="center">
  <a href="https://insideout.luthersystems.com">InsideOut</a> •
  <a href="https://luthersystems.com">Luther Systems</a> •
  <a href="https://insideout.luthersystems.com/discord"><img src="https://img.shields.io/badge/Discord-Join%20Us-5865F2?logo=discord&logoColor=white" alt="Discord"></a> •
  <a href="https://insideout.luthersystems.com/general-call"><img src="https://img.shields.io/badge/Book%20a%20Call-Schedule-4285F4?logo=google-calendar&logoColor=white" alt="Book a Call"></a>
</p>

---

## About Luther Systems

Our mission is to accelerate the advent of the automated enterprise. Founded by a team who spent years automating complex processes for some of the world's largest financial institutions, Luther Systems builds platforms that bring enterprise-grade infrastructure and operations to teams of every size.

## What is this repo?

This repository contains portable [Agent Skills](https://agentskills.io) for Luther Systems products. Each skill follows the open `SKILL.md` standard and works across any compatible AI coding tool -- no vendor lock-in.

## Available Skills

| Skill | Description |
|-------|-------------|
| **[insideout](./insideout/)** | Agentic cloud infrastructure builder & manager. Design, price, deploy, and manage AWS & GCP infrastructure through conversation. |

---

## InsideOut

InsideOut brings AI-powered cloud infrastructure management into your editor. Describe what you want to build in plain language, and **Riley** -- your AI infrastructure advisor -- guides you through selecting services, configuring them, estimating costs, generating Terraform, deploying to AWS or GCP, and managing infrastructure in production.

**No authentication or API keys required.** Install the skill and start building.

### What You Can Do

- **Describe your goal** -- tell Riley what you're building, get expert infrastructure recommendations
- **Discuss requirements** -- refine architecture, regions, compliance, scaling needs interactively
- **Estimate cost** -- see real-time monthly cost estimates as components are added or changed
- **Generate Terraform** -- production-ready, modular code with security best practices baked in
- **Deploy** -- deploy directly to AWS or GCP from the conversation
- **Operate and manage** -- inspect deployments, detect drift, roll back changes, monitor logs

### Supported Services (50+)

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

### Quick Start

Once installed, mention anything about infrastructure, cloud, AWS, GCP, Terraform, or deployment. The skill activates automatically.

```
You: "I need cloud infrastructure for a web app"

Riley: "Hi! I'm Riley, your infrastructure advisor. Tell me about the app
        you're building -- what does it do, who uses it, and what scale
        are you planning for?"

You: "It's an e-commerce platform expecting 50k monthly users on AWS"

Riley: "Great! I'd recommend ECS for your containers, RDS PostgreSQL
        for your database, ElastiCache Redis for sessions, and an ALB.
        Estimated cost: ~$350/month. Want me to adjust anything?"

You: "Looks good, generate the Terraform"

[Generates production-ready Terraform files]

You: "Deploy it"

[Deploys to AWS, streams logs in real-time]
```

### Available Tools

| Tool | Description |
|------|-------------|
| `convoopen` | Start a new infrastructure design session |
| `convoreply` | Continue the design conversation with Riley |
| `convoawait` | Wait for long-running operations |
| `convostatus` | View current components, config, and pricing |
| `tfgenerate` | Generate production-ready Terraform files |
| `tfdeploy` | Deploy generated Terraform to AWS or GCP |
| `tfplan` | Preview infrastructure changes without applying |
| `tfdestroy` | Tear down deployed infrastructure |
| `tfdrift` | Detect infrastructure drift |
| `tfstatus` | Check deployment progress |
| `tflogs` | Stream real-time deployment logs |
| `tfoutputs` | Get Terraform outputs (VPC IDs, endpoints, etc.) |
| `tfruns` | List all deployment runs for a session |
| `stackversions` | List all design versions |
| `stackdiff` | Compare two stack versions |
| `stackrollback` | Revert to a previous design version |
| `awsinspect` | Inspect deployed AWS resources |
| `gcpinspect` | Inspect deployed GCP resources |
| `credawait` | Poll for cloud credentials |
| `submit_feedback` | Submit bug reports or feature requests |
| `help` | Get workflow guidance |

### How It Works

InsideOut uses a multi-agent AI system behind a single MCP server:

| Agent | Role |
|-------|------|
| **Riley** | Infrastructure advisor -- leads the design conversation |
| **Hippo** | Cost estimation and pricing optimization |
| **Joy** | User experience and requirement gathering |
| **Etch** | Terraform code generation |
| **Core** | Architecture validation and best practices |
| **Axel** | Deployment orchestration |

The conversation flows through these agents automatically. From your perspective, you're talking to Riley -- the other agents work behind the scenes.

---

## Installation

### Using `npx skills` (recommended -- works everywhere)

```bash
# Install the InsideOut skill
npx skills add luthersystems/agent-skills --path insideout
```

This works with Claude Code, Codex, Cursor, Kiro, Windsurf, and any tool that supports the [agentskills.io](https://agentskills.io) standard.

> **Note:** Because this repo contains multiple product skills, use the `--path` flag to install a specific one.

### Using Codex

1. Install the skill:
```
$skill-installer install https://github.com/luthersystems/agent-skills/tree/main/insideout
```

2. Activate it by typing `$insideout` -- Codex will detect the missing MCP server and prompt you to install it automatically. Accept the prompt, and you're ready to go.

> **Note:** The MCP auto-install is triggered when you first invoke the skill with `$insideout`, not at skill install time. If Codex doesn't prompt, you can manually add the MCP server: `codex mcp add insideout --url https://app.luthersystems.com/v1/insideout-mcp`

### Using Claude Code

For the full Claude Code plugin experience (includes slash commands, agents, hooks, and auto-approve configuration), use the dedicated plugin:

```
/plugin marketplace add luthersystems/insideout-claude-code
/plugin install insideout
```

### Using Kiro

For the full Kiro power experience (includes steering files and auto-approve):

1. Open the Powers panel
2. Click **Add power from GitHub**
3. Enter: `luthersystems/insideout-power`

### Manual Installation

Clone and copy the skill folder to your agent's skills directory:

```bash
git clone https://github.com/luthersystems/agent-skills.git
cp -r agent-skills/insideout ~/.claude/skills/    # Claude Code
cp -r agent-skills/insideout ~/.codex/skills/     # Codex
```

## MCP Server

The InsideOut MCP server is remote (HTTPS) -- no local binary, no API keys, no authentication required. Each skill includes a `mcp.json` with the configuration:

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

Add this to your agent's MCP configuration to enable the tools.

## Platform-Specific Integrations

This repo provides the portable, cross-platform skill. For richer integrations tailored to specific tools:

| Platform | Repository | Install | Extras |
|----------|-----------|---------|--------|
| **Claude Code** | [insideout-claude-code](https://github.com/luthersystems/insideout-claude-code) | `/plugin marketplace add luthersystems/insideout-claude-code` | Plugin marketplace, `/insideout:start` command, agents, hooks, auto-approve |
| **Kiro** | [insideout-power](https://github.com/luthersystems/insideout-power) | Add power from GitHub: `luthersystems/insideout-power` | Power format, steering files, auto-approve, design pattern guides |
| **Any tool** | **This repo** | `npx skills add luthersystems/agent-skills --path insideout` | Portable skill + MCP config |

## Repository Structure

```
luthersystems/agent-skills/
├── README.md
├── LICENSE
├── insideout/
│   ├── SKILL.md          # Skill definition (triggers, tools, workflow)
│   └── mcp.json          # MCP server configuration
└── (future products)
```

Each product gets its own folder. Install individual skills with the `--path` flag:

```bash
npx skills add luthersystems/agent-skills --path insideout
```

## Links

- [InsideOut Platform](https://insideout.luthersystems.com)
- [Standalone Web App](https://insideout.luthersystemsapp.com/) -- try InsideOut without any IDE
- [Demo Video](https://insideout.luthersystems.com/demo-video)
- [Luther Systems](https://luthersystems.com)
- [Terraform Presets](https://github.com/luthersystems/insideout-terraform-presets) -- the pre-built modules InsideOut composes
- [Subreddit](https://www.reddit.com/r/luthersystems/)
- [MCP Protocol](https://modelcontextprotocol.io)
- [Agent Skills Standard](https://agentskills.io)

## Community & Support

- [Discord](https://insideout.luthersystems.com/discord) -- chat with the devs and InsideOut users
- [General Inquiry Call](https://insideout.luthersystems.com/general-call) -- book a call with us
- [Tech Call](https://insideout.luthersystems.com/tech-call) -- talk with the devs
- Email: [contact@luthersystems.com](mailto:contact@luthersystems.com)

## Contributing

1. Fork this repository
2. Create a feature branch: `git checkout -b feature/my-improvement`
3. Add or update a skill
4. Test with your preferred agent tool
5. Submit a pull request

## License

[Apache License 2.0](LICENSE)
