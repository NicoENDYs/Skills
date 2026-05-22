---
name: arch-gen
description: >
  Usar cuando el usuario pida generar, crear o escribir documentación formal de arquitectura en archivos.
  Se activa con frases como: "genera la documentación de arquitectura", "crea el arc42",
  "documenta la arquitectura del sistema", "genera los ADRs", "crea los diagramas PlantUML",
  "quiero documentación C4", "genera toda la documentación de arquitectura".
  No usar para explicaciones conversacionales (eso es arch-docs) ni para analizar el repo sin escribir archivos (eso es arch-scan).
---

# arch-gen

Genera documentación formal de arquitectura en archivos dentro de `docs/architecture/`, basándose exclusivamente en evidencia del repositorio.

## Cuándo se activa

- El usuario pide generar o crear documentación de arquitectura en archivos.
- El usuario menciona C4 model, arc42, ADRs, PlantUML o diagramas de arquitectura.
- El usuario quiere dejar documentado el sistema para el equipo o para futura referencia.
- El usuario pide un full-suite o paquete completo de documentación.

## Instrucciones

### Fase 1: Descubrimiento

Antes de escribir cualquier archivo, inspeccionar el repositorio:

Leer si existen:
- README.md, docs/, package.json, pnpm-workspace.yaml, turbo.json
- go.mod, pom.xml, Cargo.toml, docker-compose.yml, k8s/

Ejecutar para inspeccionar estructura:
- `find . -maxdepth 3 -type f \( -name "package.json" -o -name "go.mod" -o -name "docker-compose.yml" -o -name "*openapi*" -o -name "*swagger*" -o -name "*.puml" -o -name "*.env.example" \) | grep -v node_modules | head -50`
- `find . -maxdepth 3 -type d \( -name "src" -o -name "apps" -o -name "services" -o -name "packages" -o -name "internal" -o -name "cmd" -o -name "api" -o -name "infra" -o -name "k8s" -o -name "terraform" \) | grep -v node_modules | head -30`

### Fase 2: Resumen interno

Antes de generar archivos, producir un análisis breve que cubra:
- Stack y frameworks detectados
- Apps, servicios o ejecutables principales
- Almacenes de datos e infraestructura
- APIs e integraciones
- Estilo arquitectónico probable
- Riesgos, ambigüedades y brechas de información

### Fase 3: Selección de modo

Interpretar el argumento o intención del usuario:

| Modo | Acción |
|------|--------|
| `--c4-model` | Generar vistas C4 separadas |
| `--arc42` | Generar documento arc42 unificado |
| `--adr` | Generar solo Architecture Decision Records |
| `--plantuml` | Generar archivos de diagrama `.puml` |
| `--full-suite` | Generar todos los artefactos aplicables |
| Sin argumento | Preguntar al usuario qué modo prefiere o asumir `--c4-model` |

Si el usuario proporciona un scope (backend, frontend, api, auth, billing, deployment), enfocar el análisis en esa área.

### Fase 4: Generar entregables

**`--c4-model`** — crear:
- `docs/architecture/README.md`
- `docs/architecture/system-context.md`
- `docs/architecture/container-view.md`
- `docs/architecture/component-view.md`

**`--arc42`** — crear:
- `docs/architecture/arc42.md` con las 12 secciones adaptadas al repo:
  1. Introduction & Goals
  2. Constraints
  3. Context & Scope
  4. Solution Strategy
  5. Building Block View
  6. Runtime View
  7. Deployment View
  8. Cross-cutting Concepts
  9. Architecture Decisions
  10. Quality Requirements
  11. Risks & Technical Debt
  12. Glossary

**`--adr`** — crear:
- `docs/architecture/decisions/README.md`
- `docs/architecture/decisions/ADR-000-template.md`
- 1 a 3 ADRs solo si hay evidencia real de decisiones en el repo

  Formato de cada ADR:
  ```
  # ADR-NNN: [Título]
  ## Status / ## Date / ## Context
  ## Decision / ## Alternatives considered
  ## Consequences / ## Evidence
  ```

**`--plantuml`** — crear:
- `docs/architecture/diagrams/system-context.puml`
- `docs/architecture/diagrams/container-view.puml`
- `docs/architecture/diagrams/component-view.puml`

**`--full-suite`** — generar todos los artefactos anteriores.

### Fase 5: Reglas de calidad

- Usar nombres rastreables del repositorio, nunca etiquetas genéricas como "Service A".
- Marcar todo lo que no pueda verificarse: `[Observed]`, `[Inferred]`, `[Assumption]`.
- Si `docs/architecture/` ya existe, actualizar archivos en lugar de duplicar.
- Si hay poca evidencia, generar un análisis de brechas en lugar de documentación vacía.
- No describir "mejores prácticas" a menos que estén presentes o claramente ausentes en el repo.

### Fase 6: Cierre

Al terminar:
1. Listar cada archivo creado o actualizado con su ruta.
2. Resumir qué fue observado, qué fue inferido y qué permanece desconocido.
3. Sugerir próximos pasos si hay brechas de información significativas.

## Skills y comandos relacionados

- `arch-docs` — explicaciones conversacionales de arquitectura con referencias file:line
- `arch-scan` (command) — análisis del repositorio sin escribir archivos, útil antes de arch-gen
