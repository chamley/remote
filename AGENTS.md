# Project guide

Stack: Amazon Bedrock AgentCore, Strands Agents, AWS, Terraform.

## Always use current docs, never memory

AgentCore, Strands and the Terraform AWS provider change faster than training data.
Before writing or changing code that touches them, look up the current docs with
the tools below and follow them over what you remember.

| Topic | Where to look |
|---|---|
| AgentCore (Runtime, Gateway, Memory, Identity, Harness, ...) | `aws-core:amazon-bedrock` skill, then AWS MCP `search_documentation` (topic `general` / `reference_documentation`) |
| Strands Agents SDK | AWS MCP `search_documentation` with topic `strands_docs` |
| Other AWS services | the matching `aws-core:*` skill (IAM, containers, observability, ...), then AWS MCP `search_documentation` |
| What's new / recently released | AWS MCP `search_documentation` with topic `current_awareness` |
| Terraform resources & providers | `terraform` MCP: `get_latest_provider_version`, `search_providers` + `get_provider_details` for the exact resource docs; `search_modules` for modules |

## Terraform

- Look up each resource's current schema with the `terraform` MCP before writing it
  (e.g. `aws_bedrockagentcore_agent_runtime`). Don't guess argument names.
- Pin provider versions in `required_providers`; check the latest with
  `get_latest_provider_version` when adding or bumping one.
- Before reporting work done: `terraform fmt -recursive`, `terraform validate`,
  and `terraform plan` when AWS credentials are available.
- Never commit state files, `.tfvars` with secrets, or `.terraform/`.

## AWS access

- Use the AWS MCP (`run_script`) for AWS API calls, not the AWS CLI via Bash.
- Never put secrets in code or Terraform variables committed to the repo; use
  Secrets Manager / SSM and follow the `aws-core:aws-secrets-manager` skill.

## Environment

The cloud environment's setup script installs the latest stable `terraform` and
`terraform-mcp-server` from releases.hashicorp.com at session start.
