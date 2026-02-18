# Foundation Standards

## Governance Standards for a Personal AWS Project Ecosystem

This project defines and documents the standards, conventions, and governance practices that apply across a collection of interconnected AWS infrastructure and application projects. It exists to provide a single source of truth for how projects should be built, tagged, documented, and maintained.

## What Problem This Project Solves

As a personal project ecosystem grows beyond a handful of repositories, organic patterns diverge. Different projects adopt different tagging conventions, documentation structures, naming patterns, and operational practices. Without a central standard, each project makes independent decisions that accumulate into inconsistency, making cost allocation unreliable, automation fragile, and onboarding (human or AI agent) slower than necessary.

## What This Project Does

This project codifies the standards that all projects in the ecosystem should follow. It contains:

- Prescriptive standards defining what projects must do
- Rationale documents explaining why each decision was made
- Analysis documents capturing the current state that informed the standards

This project contains governance documentation only. Reusable code implementations of these standards (Terraform modules, script templates, workflow templates) belong in separate repositories.

## What This Project Changes

This project does not deploy infrastructure or modify AWS resources. It changes how projects are built and maintained by establishing authoritative standards that project maintainers and AI agents reference when creating or updating projects.

## Standards

### Resource Tagging

Standards for AWS resource tagging across all projects and IaC tools.

- [Tagging Standard](tagging/tagging-standard.md) - Prescriptive rules for required and project-specific tags
- [Tagging Rationale](tagging/tagging-rationale.md) - Reasoning behind each tagging decision
- [Tagging Analysis](tagging/tagging-analysis.md) - Current state analysis that informed the standard

## Project Ecosystem

This standard applies to projects following the naming taxonomy:

| Prefix | Visibility | Purpose |
|--------|-----------|---------|
| `foundation-*` | Public | Ecosystem-wide infrastructure and governance |
| `service-*` | Public | Functional AWS services |
| `website-*` | Public | Website infrastructure and content |
| `private-*` | Private | Personal tools, notes, and non-public work |

## Technologies Used

| Technology | Purpose | Implementation |
|------------|---------|----------------|
| Git | Version control | Standards documentation managed as code |
| Markdown | Documentation format | All standards written in Markdown |
| GitHub | Hosting and visibility | Public repository for portfolio and reference |

## License

MIT License - Stephen Abbot
