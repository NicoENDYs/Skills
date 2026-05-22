# Changelog

Todos los cambios notables de este proyecto se documentan en este archivo.

El formato se basa en [Keep a Changelog](https://keepachangelog.com/es/1.1.0/)
y el proyecto sigue [Versionado Semántico](https://semver.org/lang/es/).

## [0.1.0] - 2026-05-22

### Añadido

- Estructura inicial del repositorio como plugin de Claude Code (`endys-skills`).
- Manifiestos `.claude-plugin/plugin.json` y `.claude-plugin/marketplace.json`.
- Comando `/arch-docs` — genera documentación de arquitectura basada en evidencia
  (C4, arc42, ADRs, PlantUML).
- Comando `/arch-scan` — analiza la arquitectura del repositorio sin escribir archivos.
- Comando `/example-command` — plantilla comentada para crear nuevos comandos.
- Skill `example-skill` — plantilla comentada para crear nuevas skills.
