---
name: insideout
description: >
  Use when the user mentions infrastructure, cloud, AWS, GCP, Terraform, deployment, DevOps,
  containers, Kubernetes, serverless, VPC, networking, databases, load balancers, CDN, monitoring,
  or asks to "design infrastructure", "set up cloud", "deploy my app", "estimate cloud costs",
  "generate terraform", "inspect my deployment", "what would this cost on AWS/GCP",
  or discusses scaling, high availability, disaster recovery, or compliance requirements
  for cloud infrastructure.
version: 1.0.1
---

# InsideOut -- Agentic Infrastructure Builder & Manager

InsideOut is an agentic cloud infrastructure builder and manager by [Luther Systems](https://luthersystems.com). Describe your goal, discuss requirements, estimate cost, generate Terraform, deploy, operate and manage in production. Riley -- your AI infrastructure advisor -- guides you through the entire lifecycle on AWS and GCP.

**No authentication or API keys required.** The skill connects to a hosted MCP server.

## MCP Server

InsideOut uses a remote MCP server. Add the following to your MCP configuration:

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

No local binary, no API keys, no authentication required.

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
- **`convoreply`** -- Send user's message to Riley. Required: `session_id`, `message`.
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

**Before calling `convoopen`**, scan the user's workspace to build a `project_context` string. This gives Riley immediate context about the tech stack.

### What to Scan

| File / Pattern | Extract |
|---|---|
| `package.json`, `go.mod`, `requirements.txt`, `Cargo.toml`, `pom.xml` | Language, framework, key dependencies |
| `Dockerfile`, `docker-compose.yml` | Container usage, service topology, exposed ports |
| `*.tf`, `terraform/` | Existing IaC, provider, resource types |
| `.github/workflows/`, `.gitlab-ci.yml` | CI/CD platform and deployment targets |
| `k8s/`, `kubernetes/`, `helm/` | Kubernetes usage |
| `README.md` | Project description (first ~30 lines) |
| `.env.example` | Environment variable names (NOT values) |

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

Only include lines where something was detected.

## Conversation Flow

### Starting a Session

1. Scan workspace files silently (no output to user)
2. Call `convoopen` with `project_context` and `source`
3. Display Riley's response **verbatim** -- output the exact text from the tool response as your entire reply. No preamble, no summary, no commentary.

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
- Show Riley's messages verbatim as your entire output
- Use `convostatus` proactively to check progress
- Store the `session_id` from `convoopen` -- all tools need it
- Let users review Terraform before deploying

### Don't:
- Don't answer Riley's questions yourself -- always forward to the user
- Don't add commentary around Riley's messages
- Don't call `convoopen` more than once per session
- Don't call `tfgenerate` before design is complete
- Don't call `tfdeploy` before user reviews Terraform
- Don't fabricate session IDs -- always use the one from `convoopen`
