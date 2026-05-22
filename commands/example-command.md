---
description: Plantilla de comando — reemplaza este texto por lo que hace tu comando
argument-hint: "[describe-aqui-los-argumentos]"
allowed-tools: Read, Grep, Glob
---

# Nombre del comando

<!--
GUÍA — borra estos comentarios al crear tu comando real.

Un comando se invoca EXPLÍCITAMENTE escribiendo /nombre.
El nombre del comando = nombre del archivo sin .md
(este archivo se invoca como /example-command).

Frontmatter (todo opcional, pero recomendado):
  description    Texto mostrado en el autocompletado de "/".
  argument-hint  Pista de argumentos que ve el usuario.
  allowed-tools  Herramientas que el comando puede usar sin pedir permiso.

Variables disponibles en el cuerpo:
  $ARGUMENTS     Todo lo que el usuario escribió después de /nombre.
  $1, $2, ...    Argumentos posicionales individuales.

Sintaxis especial dentro del cuerpo:
  @ruta/archivo  Inyecta el contenido de ese archivo en el prompt.
  !`comando`     Ejecuta un comando de shell e inyecta su salida.

Cuándo usar un COMANDO en vez de una SKILL:
  - Comando: flujo que tú decides lanzar a mano (/deploy, /arch-scan).
  - Skill:   capacidad que Claude activa solo al detectar la tarea.
-->

Instrucciones para Claude cuando se invoque este comando.
Argumentos recibidos: $ARGUMENTS

## Qué hacer

1. Describe el primer paso.
2. Describe el segundo paso.
3. Describe el paso final.

## Formato de salida

Describe cómo debe responder Claude (estructura, secciones, nivel de detalle).
