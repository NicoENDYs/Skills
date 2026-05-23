name: ui-rules
description: Usar cuando el usuario pida generar, refactorizar o revisar el diseño frontend (layouts, componentes, estilos o copy) y quiera evitar que la interfaz se vea genérica o "AI-like". La skill aplica especialmente cuando se trabaja con React/Vite u otros frameworks modernos y el usuario menciona diseño, UI, UX, estilos, componentes visuales o landing pages.
---

# UI Rules — Skill anti‑diseño genérico de IA

## Propósito

Ayudar a Claude a generar y refactorizar interfaces frontend que se vean intencionales y diseñadas por humanos, evitando patrones visuales y de copy típicos de UI generada por IA (gradientes genéricos, layouts plantilla, frases vacías, etc.).

## Cuándo se activa

- Cuando el usuario pida diseñar o mejorar el diseño de una UI (frontends, landings, dashboards, formularios, layouts de componentes).
- Cuando el usuario mencione explícitamente que no quiere "diseño AI", "UI genérica" o que quiere un diseño más humano/específico.
- Cuando el usuario solicite ayuda con estilos, CSS, Tailwind, componentes UI de React/Vue/etc., sin aportar un diseño claro, y haya riesgo de caer en patrones de plantilla.

## Instrucciones

Cuando esta skill se active, Claude debe seguir estos pasos ANTES de proponer código o diseños:

1. **Clarificar contexto y objetivos**  
   - Preguntar (si no está claro) qué tipo de producto es, para quién es y cuál es el objetivo principal de la pantalla/página.  
   - Identificar si ya existe un sistema de diseño (tokens de color, tipografía, spacing, componentes) o si hay que trabajar solo con reglas generales.

2. **Aplicar reglas anti‑UI genérica de IA**  
   Claude debe seguir estas reglas de diseño de forma estricta al generar layouts, componentes y estilos:

   ### 0. Rol de la IA
   - Tratar estas reglas como constraints duras: Claude implementa, el humano diseña.  
   - NO inventar "looks" nuevos tipo SaaS genérico; solo aplicar reglas y tokens existentes.

   ### 1. Sin gradientes decorativos
   - No usar `linear-gradient` ni `radial-gradient` en botones, inputs o cards; solo fondos sólidos basados en tokens existentes.  
   - Evitar esquemas de color azul‑violeta/morado/índigo y degradados de moda tipo "neon SaaS".  
   - No usar blobs/orbs de luz ni fondos difusos abstractos.  
   - No usar `background-clip: text` con gradientes en headings.

   ### 2. Layout de features asimétrico
   - No generar secciones de 3 columnas idénticas “icono en círculo + título + descripción".  
   - Evitar iconos dentro de círculos/cuadrados de color de acento.  
   - Preferir layouts asimétricos: 2+1, bento grid, narrativo en zig‑zag, una feature principal + varias secundarias.

   ### 3. Alineación izquierda por defecto
   - Alinear a la izquierda todo body copy, descripciones y contenido de cards.  
   - Centrar solo headings muy cortos y taglines de una línea cuando tenga sentido.

   ### 4. Cards sin border de acento
   - No usar `border-left` o `border` en color primario como acento principal.  
   - Diferenciar cards por jerarquía de contenido (títulos, meta, acciones) o superficie, no por colores fuertes.

   ### 5. Border-radius con jerarquía
   - Usar radios distintos según el tamaño del componente (chips pequeños, botones medianos, cards/modales grandes).  
   - Evitar hardcodear el mismo `border-radius` en todo (especialmente 16px/9999px en masa).

   ### 6. Copy específico, no genérico
   - No usar frases vacías tipo "Empowering your journey", "Unlock the power of", "All‑in‑one solution", etc.  
   - Escribir headlines que expliquen qué hace el producto, para quién y qué resultado genera, usando vocabulario del dominio.  
   - Prohibido `Lorem ipsum`: siempre textos reales o placeholders realistas y específicos del caso.

   ### 7. Ritmo de secciones variado
   - No aplicar el mismo `padding-block` a todas las secciones.  
   - Alternar secciones densas y generosas, y variar las estructuras de grid según el contenido.

   ### 8. Tipografía, color, spacing y accesibilidad
   - Respetar tamaños mínimos (`16px` body, `12px` labels) y limitar familias/estilos de fuente.  
   - Evitar fuentes ultra‑comunes como Inter/Roboto/Montserrat/Poppins como principal, salvo que el usuario lo exija.  
   - Limitar el número de colores no neutros visibles; usar un solo color de acento y sus variantes.  
   - Usar siempre tokens de spacing (múltiplos de 4px) y mantener contrastes que cumplan WCAG AA.  
   - Asegurar `:focus-visible` y targets mínimos 44×44px en elementos interactivos.

   ### 9. Imágenes, iconos y micro‑interacciones
   - Priorizar capturas reales del producto, diagramas propios o shapes simples en lugar de ilustraciones stock genéricas.  
   - Usar un único set de iconos consistente (outline o filled, pero no mezclado).  
   - Añadir micro‑interacciones sutiles (hover, focus, active) sin animaciones exageradas ni parallax decorativo.

3. **Trabajar sección por sección**  
   - Evitar generar una landing o dashboard completo de una sola vez.  
   - Proponer primero la estructura de una sección (ej. hero), discutirla y luego pasar al código.  
   - Para cada sección, describir primero el layout en lenguaje natural (zonas, jerarquía, objetivos) y recién después mostrar el código.

4. **Ajustar al stack del usuario**  
   - Adaptar las reglas al stack indicado (React/Vite, Tailwind, CSS Modules, etc.).  
   - Usar solo los componentes que ya existan en la biblioteca del usuario (Button, Card, Section, etc.), o sugerir nuevos de forma explícita antes de usarlos.  
   - Mantener el código generado simple, legible y fácil de refactorizar por un humano.

5. **Explicar decisiones de diseño**  
   - Al final de cada propuesta de UI, incluir un breve resumen de las decisiones clave (layout, tipografía, color, ritmo) y cómo ayudan a evitar un look genérico de IA.  
   - Mencionar qué reglas se aplicaron explícitamente, para que el usuario pueda ajustar o extender este sistema de reglas.
