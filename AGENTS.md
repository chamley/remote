# Project guide

Stack: Amazon Bedrock AgentCore, Strands Agents, AWS, Terraform, GitHub, Python.

## Look up docs before writing code

Don't rely on training data for these. Check the live source first and cite the page you used.

| Topic | Source | How |
|---|---|---|
| AgentCore, Bedrock, any AWS service | AWS Knowledge MCP (`awsknowledge`, from the `aws-agents` plugin) | `search_documentation` / `read_documentation`; `retrieve_skill` for AWS guidance skills such as `amazon-bedrock`, `aws-iam`, `aws-sdk-python-usage` |
| AgentCore workflows | `aws-agents` plugin skills | `agents-get-started`, `agents-build`, `agents-connect`, `agents-deploy`, `agents-harden`, `agents-debug`, `agents-optimize` |
| Strands Agents SDK | Strands MCP (`strands`) | `search_docs` then `fetch_doc` |
| Terraform providers, modules, resource arguments | Terraform MCP (`terraform`) | `get_latest_provider_version`, then `get_provider_capabilities` / `get_provider_details`. No HCP token is set, so it's registry docs only |
| Terraform style, tests, modules, imports | HashiCorp `terraform@hashicorp` plugin skills | `terraform-style-guide`, `terraform-test`, `refactor-module`, `terraform-search-import` |
| GitHub (Actions, CLI, API) | docs.github.com | Fetch `https://docs.github.com/llms.txt` for the page index. Use `gh help <cmd>` for CLI flags |
| Python | docs.python.org and the library's own docs | No official docs plugin. Check the installed package version before you use an API |

If the `aws-core` plugin is also installed, its `aws-mcp` server offers the same
AWS docs tools; prefer `awsknowledge` for docs. Use `aws-core`'s `run_script`
only for real AWS API calls, and only once AWS access has been set up.

## Terraform

- Look up each resource's current schema with the `terraform` MCP before writing it
  (e.g. `aws_bedrockagentcore_agent_runtime`). Don't guess argument names.
- Pin provider versions in `required_providers`; check the latest with
  `get_latest_provider_version` when adding or bumping one.
- Before reporting work done: `terraform fmt -recursive`, `terraform validate`,
  and `terraform plan` when AWS credentials are available.
- Never commit state files, `.tfvars` with secrets, or `.terraform/`.

## Secrets

Never put secrets in code or in committed Terraform variables; use Secrets Manager
or SSM Parameter Store.

## Where this setup lives

- `.claude/settings.json`: plugin marketplaces and enabled plugins (`aws-agents`, `terraform@hashicorp`).
- `.mcp.json`: `terraform` (native `terraform-mcp-server`) and `strands` (`uvx strands-agents-mcp-server`).
- Cloud environment setup script: installs the latest stable `terraform` and
  `terraform-mcp-server` at session start. The environment allows
  `registry.terraform.io`, `releases.hashicorp.com` and `strandsagents.com`.
