---
description: Generate evidence-based architecture documentation from repository analysis (C4, arc42, ADRs, PlantUML)
argument-hint: "[--c4-model | --arc42 | --adr | --plantuml | --full-suite] [optional scope: backend | frontend | api | auth | billing | deployment]"
allowed-tools: Read, Write, Edit, Bash
---

# Architecture Documentation Generator

Generate architecture documentation for this repository.
Mode and scope: $ARGUMENTS

## Operating rules

- Base every conclusion on repository evidence. Do not invent services, integrations, databases, queues, or external systems.
- When something seems likely but cannot be verified, label it `[Inferred]`.
- When something is assumed without evidence, label it `[Assumption]`.
- Prefer concise, maintainable documentation over exhaustive generic templates.
- Write all outputs under `docs/architecture/`.
- If `docs/architecture/` already exists, update existing files instead of duplicating content.
- If the selected mode lacks enough evidence, generate a gap analysis and a minimal draft only.
- Do not describe "best practices" unless they are clearly present or clearly missing in the repo.

***

## Phase 1: Discovery

Run these before writing any documentation.

### Read key files (if they exist)
- @README.md
- @docs/
- @package.json
- @pnpm-workspace.yaml
- @turbo.json
- @go.mod
- @pom.xml
- @Cargo.toml
- @docker-compose.yml
- @k8s/

### Inspect structure
- !`find . -maxdepth 3 -type f \( -name "package.json" -o -name "go.mod" -o -name "pom.xml" -o -name "Cargo.toml" -o -name "docker-compose.yml" -o -name "*.puml" -o -name "*openapi*" -o -name "*swagger*" -o -name "*.env.example" \) | grep -v node_modules | head -50`
- !`find . -maxdepth 3 -type d \( -name "src" -o -name "apps" -o -name "services" -o -name "packages" -o -name "internal" -o -name "cmd" -o -name "api" -o -name "infra" -o -name "k8s" -o -name "terraform" -o -name "deploy" \) | grep -v node_modules | head -30`

***

## Phase 2: Architecture summary

Before generating any file, produce a brief internal analysis covering:

- Detected stack and frameworks
- Main executables / apps / services
- Data stores and infrastructure clues
- API surface and integrations
- Likely architectural style (monolith / modular monolith / microservices / serverless)
- Risks, ambiguities, and information gaps

If a scope was provided in $ARGUMENTS, focus the entire analysis on that bounded area.

***

## Phase 3: Mode selection

Interpret $ARGUMENTS:

| Flag | Action |
|------|--------|
| `--c4-model` | Generate C4-based structural documentation |
| `--arc42` | Generate arc42-style unified documentation |
| `--adr` | Generate Architecture Decision Records only |
| `--plantuml` | Generate PlantUML diagram source files only |
| `--full-suite` | Generate all applicable artifacts |
| _(no flag)_ | Default to `--c4-model` |

***

## Deliverables by mode

### `--c4-model`

Create:
- `docs/architecture/README.md` — overview and index
- `docs/architecture/system-context.md` — system purpose, actors, external systems
- `docs/architecture/container-view.md` — containers/services, responsibilities, interfaces
- `docs/architecture/component-view.md` — internal modules, dependencies, design patterns

Each file must include:
- Evidence-based descriptions (label inferences clearly)
- ASCII or Mermaid diagram if structure is clear enough
- Assumptions section at the bottom

***

### `--arc42`

Create:
- `docs/architecture/arc42.md`

Include all 12 arc42 sections adapted to the repository. Keep each section concise — omit sections where there is no evidence:

1. **Introduction & Goals** — system purpose, stakeholders, top quality goals
2. **Constraints** — technical, organizational, or business constraints observed
3. **Context & Scope** — system boundaries, external interfaces, actors
4. **Solution Strategy** — foundational decisions: stack, patterns, architectural style
5. **Building Block View** — modules, components, responsibilities, relationships
6. **Runtime View** — key execution flows and sequences with evidence
7. **Deployment View** — infrastructure, Docker, Kubernetes, cloud clues
8. **Cross-cutting Concepts** — auth, logging, error handling, security, i18n
9. **Architecture Decisions** — key decisions with context and alternatives
10. **Quality Requirements** — scalability, availability, performance, maintainability
11. **Risks & Technical Debt** — prioritized list of observed risks and debt
12. **Glossary** — domain and technical terms used in the project

***

### `--adr`

Create:
- `docs/architecture/decisions/README.md` — ADR index and process guide
- `docs/architecture/decisions/ADR-000-template.md` — blank template

Then generate 1–3 ADRs only if there is clear repository evidence for real decisions. Do not generate ADRs for hypothetical decisions.

Possible ADR candidates (only if evidence exists):
- Framework or language selection
- Monolith vs microservices vs modular monolith
- Database or data store choice
- Authentication and authorization strategy
- Deployment and infrastructure approach
- Messaging or event-driven patterns

Each ADR format:

```
# ADR-NNN: [Short title]

## Status
[Proposed | Accepted | Deprecated | Superseded by ADR-NNN]

## Date
[YYYY-MM-DD]

## Context
[What situation or problem led to this decision]

## Decision
[What was decided and why]

## Alternatives considered
[Other options evaluated and why they were not chosen]

## Consequences
[Positive and negative outcomes of this decision]

## Evidence
[Files, configs, or code that support this ADR]
```

***

### `--plantuml`

Create:
- `docs/architecture/diagrams/system-context.puml`
- `docs/architecture/diagrams/container-view.puml`
- `docs/architecture/diagrams/component-view.puml`

Rules for diagrams:
- Use names that match repository evidence exactly
- Keep diagrams minimal and readable
- Add notes where assumptions are required
- Use C4-PlantUML macros if applicable (`!include C4Context.puml`)
- Label inferred elements with a note

***

### `--full-suite`

Generate all of the following:
- C4 docs (system-context, container-view, component-view)
- arc42.md (abbreviated version, not full detail)
- PlantUML diagrams
- ADR template + 1–3 evidence-based ADRs

***

## Quality bar

- Use repository-traceable names, not generic labels like "Service A" or "Module 1".
- Separate observed facts from inferences: mark with `[Observed]`, `[Inferred]`, `[Assumption]`.
- Keep each file skimmable: use headings, short paragraphs, and bullet lists.
- Document architectural debt separately from architecture facts.
- Do not pad sections — a short accurate section is better than a long invented one.

***

## Final step

After generating all files:
1. List every file created or updated with its path.
2. Summarize what was observed, what was inferred, and what remains unknown.
3. Suggest next steps if information gaps are significant.
