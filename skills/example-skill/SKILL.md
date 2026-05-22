---
name: example-skill
description: Plantilla de skill — Claude la invoca automáticamente cuando esta descripción coincide con la tarea actual. Reemplaza este texto por las condiciones reales de activación, empezando por "Usar cuando...".
---

# Nombre de la skill

<!--
GUÍA — borra estos comentarios al crear tu skill real.

Diferencia clave con un comando:
  - Un COMANDO lo invocas TÚ escribiendo /nombre.
  - Una SKILL la invoca CLAUDE por su cuenta, cuando detecta que el
    campo `description` del frontmatter coincide con la tarea actual.

Por eso la `description` es lo más importante: es el DISPARADOR.
Escríbela siendo específico sobre las situaciones que deben activarla
(por ejemplo: "Usar cuando el usuario pida revisar seguridad de un PR").

El frontmatter de una skill SOLO admite dos campos:
  name         Identificador en kebab-case (debe coincidir con la carpeta).
  description  Cuándo se activa + qué hace. Es el disparador.

Estructura en disco de una skill:
  skills/
    mi-skill/
      SKILL.md        Obligatorio (este archivo).
      references/     Opcional: documentación de apoyo que Claude lee si la necesita.
      scripts/        Opcional: scripts ejecutables.
      assets/         Opcional: plantillas, imágenes u otros recursos.
-->

## Propósito

Explica en una o dos frases qué problema resuelve esta skill.

## Cuándo se activa

- Situación 1 en la que esta skill aplica.
- Situación 2 en la que esta skill aplica.

## Instrucciones

Pasos que Claude debe seguir cuando la skill se activa:

1. Primer paso.
2. Segundo paso.
3. Paso final.
