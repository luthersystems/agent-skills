---
name: insideout
description: >
  Use when the user mentions infrastructure, cloud, AWS, GCP, Terraform, deployment, DevOps,
  containers, Kubernetes, serverless, VPC, networking, databases, load balancers, CDN, monitoring,
  or asks to "design infrastructure", "set up cloud", "deploy my app", "estimate cloud costs",
  "generate terraform", "inspect my deployment", "what would this cost on AWS/GCP",
  or discusses scaling, high availability, disaster recovery, or compliance requirements
  for cloud infrastructure.
metadata:
  short-description: Design, deploy & manage cloud infrastructure
version: 1.0.1
---

# InsideOut -- Agentic Infrastructure Builder & Manager

InsideOut is an agentic cloud infrastructure builder and manager by [Luther Systems](https://luthersystems.com). Describe your goal, discuss requirements, estimate cost, generate Terraform, deploy, operate and manage in production. Riley -- your AI infrastructure advisor -- guides you through the entire lifecycle on AWS and GCP.

**No authentication or API keys required.** The skill connects to a hosted MCP server.

**Important:** If any guidance in this skill conflicts with the descriptions on the MCP tools themselves, follow the MCP tool descriptions -- they are the authoritative source and are kept up to date with the server.

## MCP Server

InsideOut uses a remote MCP server. No local binary, no API keys, no authentication required.

### If MCP server is not connected

Before proceeding with any InsideOut workflow, check if the `insideout` MCP tools (like `convoopen`, `convoreply`) are available. If they are not, the MCP server needs to be added. Run this command in the shell:

```bash
codex mcp add insideout --url https://app.luthersystems.com/v1/insideout-mcp
```

For other agents, add this to your MCP configuration:

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

After adding the MCP server, the InsideOut tools will be available immediately.

## When This Skill Activates

Use InsideOut when the user's request involves:
- Designing cloud infrastructure (AWS or GCP)
- Estimating cloud costs
- Generating Terraform code
- Deploying infrastructure
- Inspecting deployed cloud resources
- Comparing AWS vs GCP options
- Infrastructure planning for their application
- Managing, updating, or rolling back existing deployments

## Available MCP Tools

### Design Conversation
- **`convoopen`** -- Start a new session. Pass `source` and `project_context` (see below).
- **`convoreply`** -- Send user's message to Riley. Required: `session_id`, `message`. Do not resend `project_context` if it was already provided in `convoopen`. Only pass new context if the user reveals something genuinely new about their project.
- **`convoawait`** -- Wait for long-running response. Required: `session_id`.
- **`convostatus`** -- View current stack (components, config, pricing). Required: `session_id`.

### Terraform Operations
- **`tfgenerate`** -- Generate production-ready Terraform files. Required: `session_id`.
- **`tfdeploy`** -- Deploy to AWS or GCP. Required: `session_id`. Takes 15+ minutes.
- **`tfplan`** -- Preview changes without applying. Required: `session_id`.
- **`tfdestroy`** -- Tear down deployed infrastructure. Required: `session_id`.
- **`tfdrift`** -- Detect infrastructure drift. Required: `session_id`.

### Monitoring
- **`tfstatus`** -- Quick deployment status check. Required: `session_id`.
- **`tflogs`** -- Stream deployment logs (paginated). Required: `session_id`.
- **`tfoutputs`** -- Get Terraform outputs (VPC IDs, endpoints, etc.). Required: `session_id`.
- **`tfruns`** -- List all deployment runs for a session. Required: `session_id`.

### Stack Management
- **`stackversions`** -- List all design versions (draft/confirmed/applied). Required: `session_id`.
- **`stackdiff`** -- Compare two stack versions. Required: `session_id`.
- **`stackrollback`** -- Revert to a previous design version. Required: `session_id`.

### Cloud Inspection
- **`awsinspect`** -- Inspect deployed AWS resources. Required: `session_id`.
- **`gcpinspect`** -- Inspect deployed GCP resources. Required: `session_id`.

### Utility
- **`credawait`** -- Poll for cloud credentials after browser-based connection. Required: `session_id`.
- **`submit_feedback`** -- Submit bug reports or feature requests. Required: `session_id`, `category`, `message`.
- **`help`** -- Get workflow guidance and tool documentation.

## Supported Services (50+)

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

## Workspace Context Scanning

Before calling `convoopen`, you may scan the user's workspace to build a `project_context` string. This gives Riley immediate context about the tech stack so it can skip discovery questions.

### User consent required

**Ask the user before scanning.** Tell them you'd like to scan workspace files for general tech stack info (language, framework, cloud provider) and show them a summary of what will be sent before calling `convoopen`. If the user declines, start the session without `project_context` -- Riley will ask discovery questions instead.

### What to Scan

| File / Pattern | Extract |
|---|---|
| `package.json`, `go.mod`, `requirements.txt`, `Cargo.toml`, `pom.xml` | Language, framework, key dependencies |
| `Dockerfile`, `docker-compose.yml` | Container usage, service topology, exposed ports |
| `*.tf`, `terraform/` | Existing IaC, provider, resource types |
| `.github/workflows/`, `.gitlab-ci.yml` | CI/CD platform and deployment targets |
| `k8s/`, `kubernetes/`, `helm/` | Kubernetes usage |
| `README.md` | Project description (first ~30 lines) |

### What to NEVER include

- **Credentials or secrets** -- Never read `.env`, `.env.local`, API keys, tokens, private keys, or credential files
- **PII** -- No usernames, emails, or personally identifiable information
- **Source code** -- Do not include file contents, only metadata summaries
- **Internal URLs or IPs** -- Omit specific internal hostnames, IPs, or endpoint URLs

### Cloud Provider Detection

| Signal | Provider |
|---|---|
| `provider "aws"` in `.tf`, `aws-sdk`/`boto3` in deps | AWS |
| `provider "google"` in `.tf`, `@google-cloud/*` in deps | GCP |

### Format

```
Language/Runtime: Node.js 20, TypeScript
Framework: Next.js 15
Databases/Services: PostgreSQL (via prisma), Redis (via ioredis)
Target Cloud: AWS (existing Terraform with ECS + RDS resources)
Infrastructure: Docker Compose (3 services), Terraform
CI/CD: GitHub Actions
```

Only include lines where something was detected. Keep it general and anonymized.

## Conversation Flow

### Starting a Session

1. Ask the user if you can scan their workspace for general tech stack info (see Workspace Context Scanning above). If they agree, scan and show them the summary before proceeding. If they decline, skip `project_context`.
2. Call `convoopen` with:
   - `project_context`: The anonymized context string you built from scanning (omit if the user declined). Must not contain credentials, secrets, PII, source code, or internal URLs.
   - `source`: Set this to the IDE/agent platform. Accepted values: `"claude-code"`, `"kiro"`, `"cursor"`, `"vscode"`, `"windsurf"`, `"web"`. Defaults to `"mcp"` if omitted. This controls the credential connect screen UI. For platforms not in this list (e.g. Codex), use `"web"`.
3. Display Riley's message to the user. The tool response contains delimiters like `=== Riley ===`, `== Message ==`, `== End ==`, `=== End ===`. **Strip all of these delimiters** -- only show the actual message content between them. Do not add any preamble, summary, or commentary of your own.

### You are a transparent relay

The user is talking to Riley, not to you. Your only job is to pass messages between the user and Riley via the MCP tools. Never add filler like "I'm forwarding your message to Riley", "Here's what Riley said", "Let me ask Riley about that", or any other commentary before or after Riley's responses. Just show Riley's message as your entire output.

### During the Conversation

**Every user message must produce a tool call.** Apply this decision tree:

1. Is the user responding to Riley? -> `convoreply`
2. Is the user asking for Terraform? -> `tfgenerate` (if `[TERRAFORM_READY: true]`)
3. Is the user asking to deploy? -> `tfdeploy`
4. Is the user asking for status? -> `convostatus`, `tfstatus`, or `tflogs`
5. Not sure? -> Default to `convoreply`

### Handling Timeouts

Riley's responses can take 20-60 seconds. When `convoreply` returns a `"processing"` status:

1. **Call `convoawait`** with the same `session_id`
2. **NEVER call `convoreply` again with the same message** -- this sends a duplicate
3. If `convoawait` also times out, call it again (idempotent and safe to retry)

### Phase Transitions

| Phase | Signal | Action |
|---|---|---|
| Design | Riley asking questions | Forward to user via `convoreply` |
| Design complete | `[TERRAFORM_READY: true]` | Call `tfgenerate` |
| Terraform generated | Files returned | Show files, offer `tfdeploy` |
| Deploying | Job running | Monitor with `tfstatus` / `tflogs` |
| Deployed | Status complete | Verify with `awsinspect` / `gcpinspect` |

**Internal signals like `[TERRAFORM_READY: true]` are for routing only -- never show them to the user.**

## Critical Rules

### Do:
- Show Riley's messages as your entire output, but **strip the delimiter lines** (`=== Riley ===`, `== Message ==`, `== End ==`, `=== End ===`) -- only display the content between them
- Use `convostatus` proactively to check progress
- Store the `session_id` from `convoopen` -- all tools need it
- Let users review Terraform before deploying

### Don't:
- Don't answer Riley's questions yourself -- always forward to the user
- Don't add commentary around Riley's messages -- no "I'm forwarding this", "Here's Riley's response", "Let me pass that along", "I'm sending that confirmation to Riley", or predictions like "the next response should move into cost". You are invisible. Call the tool silently and show only Riley's output.
- Don't call `convoopen` more than once per session
- Don't call `tfgenerate` before design is complete
- Don't call `tfdeploy` before user reviews Terraform
- Don't fabricate session IDs -- always use the one from `convoopen`
