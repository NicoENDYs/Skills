---
description: Scan repository structure and produce an architecture analysis without generating documentation
argument-hint: "[optional scope: backend | frontend | api | auth | billing]"
allowed-tools: Read, Bash
---

# Architecture Scanner

Scan and analyze the repository architecture. Scope (if provided): $ARGUMENTS

## Rules

- Read only. Do not create or modify files.
- Do not invent components, services, or integrations not visible in the code.
- Mark all inferences with [Inferred] and unverified assumptions with [Assumption].

## Discovery steps

### Read key files
- @README.md
- @package.json
- @go.mod
- @pom.xml
- @Cargo.toml
- @docker-compose.yml

### Inspect file tree
- !`find . -maxdepth 3 -type f \( -name "package.json" -o -name "go.mod" -o -name "docker-compose.yml" -o -name "*openapi*" -o -name "*swagger*" -o -name "*.puml" \) | head -40`
- !`find . -maxdepth 3 -type d \( -name "src" -o -name "apps" -o -name "services" -o -name "packages" -o -name "internal" -o -name "cmd" -o -name "api" -o -name "infra" -o -name "k8s" \) | head -30`

## Output format

Return a concise markdown report with the following sections:

### Stack
- Languages, frameworks, runtimes detected

### Structure
- Executables / apps / services
- Shared libraries or packages
- Monorepo or polyrepo pattern

### Data layer
- Databases, caches, object storage [with evidence]

### Infrastructure
- Docker, Kubernetes, cloud provider clues [with evidence]

### APIs and integrations
- HTTP endpoints, message queues, external services [with evidence]

### Architecture style
- Likely pattern: monolith / modular monolith / microservices / serverless / other
- Evidence for this classification

### Risks and technical debt
- Missing tests, tightly coupled modules, unclear boundaries, etc.

### Information gaps
- What is unclear or missing that would improve documentation quality

### Recommended documentation mode
- Suggest one of: `--c4-model`, `--arc42`, `--adr`, `--plantuml`, `--full-suite`
- Explain why

## Final instruction

After the report, suggest the exact command to run next, for example:
`/arch-docs --c4-model`
