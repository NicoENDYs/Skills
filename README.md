# endys-skills

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Claude Code Plugin](https://img.shields.io/badge/Claude%20Code-plugin-7c3aed.svg)](https://docs.claude.com/en/docs/claude-code/plugins)

Colección personal de **comandos** y **skills** de [Claude Code](https://claude.com/claude-code) de NicoENDYs.

El repositorio está empaquetado como un **plugin de Claude Code**, por lo que se
instala con un comando y queda disponible en todos tus proyectos.

---

## ¿Qué es esto?

Claude Code permite extender su comportamiento con dos mecanismos:

| Tipo        | Cómo se activa                                              | Ejemplo de uso          |
|-------------|-------------------------------------------------------------|-------------------------|
| **Comando** | Lo invocas tú escribiendo `/nombre`.                        | `/arch-scan`            |
| **Skill**   | La invoca Claude solo cuando detecta que aplica a la tarea. | Revisar seguridad de PR |

Este repo agrupa ambos en un solo plugin instalable.

---

## Instalación

### Como plugin (recomendado)

Dentro de Claude Code:

```
/plugin marketplace add NicoENDYs/Skills
/plugin install endys-skills@endys
```

La primera línea registra este repositorio como marketplace; la segunda instala
el plugin. A partir de ahí los comandos y skills están disponibles en cualquier
proyecto.

Para actualizar a una versión nueva:

```
/plugin marketplace update endys
```

### Instalación manual (alternativa)

Si prefieres no usar el sistema de plugins, copia los archivos sueltos al
directorio `.claude/` de tu proyecto:

```bash
mkdir -p .claude/commands .claude/skills
cp commands/*.md       .claude/commands/
cp -r skills/*         .claude/skills/
```

---

## Catálogo

> Este catálogo se mantiene a mano. Al añadir un comando o skill, actualízalo aquí.

### Comandos

| Comando            | Descripción                                                              |
|--------------------|--------------------------------------------------------------------------|
| `/arch-scan`       | Analiza la arquitectura del repositorio y produce un reporte. No escribe archivos. |
| `/arch-docs`       | Genera documentación de arquitectura basada en evidencia (C4, arc42, ADRs, PlantUML). |
| `/example-command` | Plantilla comentada para crear nuevos comandos.                          |

### Skills

| Skill           | Descripción                                      |
|-----------------|--------------------------------------------------|
| `example-skill` | Plantilla comentada para crear nuevas skills.    |

---

## Uso rápido

Flujo recomendado para documentar arquitectura:

```
/arch-scan
```

Analiza el repo y recomienda qué documentación generar. No escribe nada.

```
/arch-docs --arc42
/arch-docs --c4-model
/arch-docs --adr
/arch-docs --plantuml
/arch-docs --full-suite
```

Cada modo escribe sus artefactos en `docs/architecture/`. Cualquier modo acepta
un *scope* opcional para enfocar el análisis:

```
/arch-docs --c4-model backend
/arch-docs --arc42 auth
```

---

## Cómo añadir un nuevo comando

1. Crea `commands/mi-comando.md`. Puedes partir de `commands/example-command.md`.
2. El nombre del archivo es el nombre del comando: `mi-comando.md` → `/mi-comando`.
3. Define el frontmatter (`description`, `argument-hint`, `allowed-tools`).
4. Escribe en el cuerpo las instrucciones para Claude.
5. Añádelo a la tabla del [Catálogo](#catálogo) y al [CHANGELOG](CHANGELOG.md).

## Cómo añadir una nueva skill

1. Crea la carpeta `skills/mi-skill/` con un archivo `SKILL.md` dentro.
   Puedes partir de `skills/example-skill/`.
2. El frontmatter de la skill solo admite `name` y `description`.
3. La `description` es el **disparador**: descríbela con precisión, porque
   determina cuándo Claude activa la skill por su cuenta.
4. Añade archivos de apoyo en `references/`, `scripts/` o `assets/` si los necesitas.
5. Añádela a la tabla del [Catálogo](#catálogo) y al [CHANGELOG](CHANGELOG.md).

---

## Estructura del repositorio

```
Skills/
├── .claude-plugin/
│   ├── plugin.json          Manifiesto del plugin (nombre, versión, autor).
│   └── marketplace.json     Hace el repo instalable vía /plugin marketplace.
├── commands/                Comandos invocables con /nombre.
│   ├── arch-docs.md
│   ├── arch-scan.md
│   └── example-command.md
├── skills/                  Skills que Claude activa automáticamente.
│   └── example-skill/
│       └── SKILL.md
├── CHANGELOG.md
├── LICENSE
└── README.md
```

---

## Licencia

Distribuido bajo licencia [MIT](LICENSE).
