# Tagging Standard

## Overview

All AWS resources created by projects in this ecosystem must be tagged. Tags enable cost allocation, resource discovery, operational management, audit trails, and compliance verification. This standard is implementation-agnostic and applies equally to resources created by Terraform, CloudFormation, Bash scripts, or any other tool.

## Tag Key and Value Conventions

### Tag Keys

- PascalCase with no spaces or delimiters (e.g., `CostCenter`, `DeployedBy`, `LastDeployed`)
- No abbreviations unless universally understood (e.g., `Id` is acceptable)
- Consistent casing: `AccountId` not `AccountID`

### Tag Values

- Lowercase with hyphens for names and identifiers (e.g., `service-ephemeral-splunk`, `us-east-1`, `prd`)
- Values that have a natural format follow that format (email addresses, ARNs, URLs, ISO timestamps)
- No trailing whitespace, no empty values except where explicitly permitted (e.g., `AccountAlias`)

## Required Tags

Required tags apply to all projects and all resources. They are project-agnostic.

### Static Tags

Static tags are configured in environment files or hardcoded, and remain constant across deployments unless explicitly updated.

| Tag | Source | Format | Example |
|-----|--------|--------|---------|
| **ContactEmail** | config.env | Email address | `stephen@denverbytes.com` |
| **CostCenter** | config.env | Lowercase string | `default` |
| **Environment** | config.env | Lowercase 3-character code | `prd` |
| **ManagedBy** | Hardcoded per tool | Tool name, PascalCase | `OpenTofu`, `CloudFormation`, `Bash` |
| **Owner** | config.env | PascalCase, no spaces | `StephenAbbot`, `PlatformTeam` |

#### Environment Values

The `Environment` tag uses a closed set of lowercase 3-character codes:

| Value | Meaning |
|-------|---------|
| `prd` | Production |
| `stg` | Staging |
| `tst` | Test |
| `dev` | Development |

### Dynamic Tags

Dynamic tags are computed at deployment time and reflect current deployment context.

| Tag | Source | Format | Example |
|-----|--------|--------|---------|
| **AccountAlias** | AWS account alias | Lowercase string, blank if not set | `stephenabbot-base` |
| **AccountId** | AWS STS GetCallerIdentity | 12-digit number | `694394480102` |
| **DeployedBy** | AWS STS GetCallerIdentity | IAM principal ARN | `arn:aws:sts::694394480102:assumed-role/role/session` |
| **LastDeployed** | System clock at deployment | ISO 8601 UTC | `2026-02-17T13:49:00Z` |
| **ProjectName** | Full Git remote URL | Repo name, lowercase-with-hyphens | `service-ephemeral-splunk` |
| **Region** | AWS region or `global` | AWS region code | `us-east-1`, `global` |
| **ProjectRepository** | Git remote URL | Full HTTPS URL | `https://github.com/stephenabbot/service-ephemeral-splunk` |

## Project-Specific Tags

Project-specific tags are additional tags required by the context of a particular project. They are defined in the project's own documentation and supplement the required tags.

## Tag Application Rules

1. Every AWS resource must have all 12 required tags
2. Dynamic tags must be computed at deployment time, not hardcoded
3. `LastDeployed` must be updated on every deployment, not just initial creation
4. `AccountAlias` must be blank (empty string) if the account has no alias configured, not omitted
5. `Region` must be `global` for global resources (IAM, Route53, CloudFront), not the region the deployment runs from
6. `Project` must match the git repository name exactly
7. Project-specific tags are documented in each project's `docs/` directory
8. The `Name` tag is recommended for all resources for AWS Console readability but is not part of this standard

## Configuration

### config.env Pattern

Static tag values are sourced from a `config.env` file in each project root:

```bash
TAG_CONTACT_EMAIL=stephen@denverbytes.com
TAG_COST_CENTER=default
TAG_ENVIRONMENT=prd
TAG_OWNER=StephenAbbot
```

### Dynamic Tag Generation

Dynamic tag values are computed using standard tooling:

```bash
# Project and Repository
git remote get-url origin                                          # Repository
git remote get-url origin | sed 's/.*\///' | sed 's/\.git$//'     # Project

# AWS identity
aws sts get-caller-identity --query 'Account' --output text       # AccountId
aws sts get-caller-identity --query 'Arn' --output text           # DeployedBy
aws iam list-account-aliases --query 'AccountAliases[0]' --output text  # AccountAlias

# Region and timestamp
aws configure get region                                           # Region
date -u +"%Y-%m-%dT%H:%M:%SZ"                                    # LastDeployed
```
