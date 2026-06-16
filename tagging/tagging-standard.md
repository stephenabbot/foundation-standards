# Tagging Standard

## Overview

All AWS resources created by projects in this ecosystem must be tagged. Tags enable cost allocation, resource discovery, and operational traceability.

## Tag Key and Value Conventions

### Tag Keys

- PascalCase, no spaces or delimiters (`ProjectName`, `ManagedBy`)
- No abbreviations unless universally understood

### Tag Values

- Identifiers use lowercase with hyphens (`service-ephemeral-splunk`, `prd`)
- Values with a natural format follow that format (ARNs, URLs)

## Required Tags

Six tags are required on all AWS resources.

| Tag | Value | Source | Example |
|-----|-------|--------|---------|
| `ProjectName` | Repository name | Derived from git remote | `service-ephemeral-splunk` |
| `ProjectRepository` | Full repository URL | Git remote URL | `https://github.com/stephenabbot/service-ephemeral-splunk` |
| `ManagedBy` | Deployment tool | Hardcoded per project | `OpenTofu`, `CloudFormation`, `Bash` |
| `Environment` | Deployment environment | `config.env` | `prd` |
| `DeployedBy` | IAM principal ARN | AWS STS at deploy time | `arn:aws:sts::694394480102:assumed-role/role/session` |
| `AccountAlias` | AWS account alias | AWS IAM at deploy time | `stephenabbot-base` |

### Environment Values

The `Environment` tag uses a closed set of lowercase 3-character codes:

| Value | Meaning |
|-------|---------|
| `prd` | Production |
| `stg` | Staging |
| `tst` | Test |
| `dev` | Development |

### ManagedBy Values

| Value | When to use |
|-------|-------------|
| `OpenTofu` | Resources deployed via OpenTofu |
| `CloudFormation` | Resources deployed via CloudFormation |
| `Bash` | Resources deployed via shell scripts |

## Configuration

### config.env Pattern

Static tag values are sourced from a `config.env` file in the project root:

```bash
TAG_ENVIRONMENT=prd
```

### Computing Tags at Deploy Time

```bash
# Project identity
ProjectName=$(git remote get-url origin | sed 's/.*\///' | sed 's/\.git$//')
ProjectRepository=$(git remote get-url origin)

# AWS identity
DeployedBy=$(aws sts get-caller-identity --query 'Arn' --output text)
AccountAlias=$(aws iam list-account-aliases --query 'AccountAliases[0]' --output text)
```

## Project-Specific Tags

Projects may define additional tags in their own documentation. Additional tags supplement the required set and follow the same key and value conventions.
