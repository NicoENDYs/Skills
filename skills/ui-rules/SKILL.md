---
name: ui-rules
description: >
  Usar cuando el usuario pida generar, refactorizar o revisar diseño frontend
  (layouts, componentes, estilos, copy) en proyectos React/Vite + Tailwind 4 +
  tokens CSS. Aplica tanto a UI de producto (dashboards, formularios, tablas)
  como a páginas de marketing (landings, pricing). Activar especialmente si el
  usuario menciona: diseño, UI, UX, estilos, componentes visuales, landing pages,
  o cuando haya riesgo de caer en patrones "AI-like" o "template SaaS".
---

# Reglas de Diseño Frontend — Anti-UI Genérica de IA

Documento operativo: define qué puede y qué no puede hacer la IA al generar interfaces, qué tokens debe usar y cómo verificar el resultado. Stack objetivo: React/Vite + Tailwind 4 + tokens CSS. Aplica a UI de producto (dashboards, formularios, tablas) y a páginas de marketing.

---

## 0. Cómo trabajar con IA en este sistema

Esta sección es contractual: define el rol de la IA y el flujo de trabajo. Si algo de aquí se rompe, el resto de las reglas dejan de aplicar bien.

### 0.1 Rol de la IA

- La IA es **implementador**, no director creativo. No decide el estilo visual global; consume tokens, componentes y layouts ya definidos.
- **Prompts prohibidos:** "haz que se vea bonito", "modern UI", "diseño minimal SaaS", "como Tailwind UI / Vercel / Linear".
- Cada prompt debe especificar: (a) qué sección se está construyendo, (b) qué componentes existentes puede usar, (c) qué tokens están disponibles, (d) cuál es el objetivo de la sección.
- Si la IA necesita un componente o patrón que no existe, lo describe primero en texto para aprobación. **No inventa.**

### 0.2 Flujo de trabajo

- Trabajar sección por sección, no la página completa en un solo prompt.
- Antes de pedir UI nueva, definir 3-5 referencias concretas (capturas) explicando qué tomar de cada una: layout, spacing, jerarquía, tono.
- La IA produce el primer draft estructural. El ajuste fino (spacing, copy, jerarquía) se hace manualmente o en iteraciones cortas.
- Al finalizar, la IA corre el checklist de auto-auditoría (sección 8) sobre su propio output.

### 0.3 Sistema de componentes permitidos

- La IA solo usa componentes presentes en la biblioteca: `Button`, `Card`, `Section`, `Stack`, `Grid`, `Badge`, `Input`, `Table`, `StatCard`, `EmptyState`, `Skeleton` (lista a mantener en `components/README.md`).
- Cada componente tiene props documentadas y variantes cerradas. La IA no agrega variantes nuevas sin aprobación.
- Los componentes no aceptan valores arbitrarios de spacing/color/radius: **solo tokens**.

---

## 1. Reglas críticas

Estas son las que rompen la sensación de "template AI" en primera mirada. Si una sola falla, el resultado se siente genérico aunque todo lo demás esté bien.

### Regla 1.1 — Sin gradientes decorativos

**Prohibido:**
- `linear-gradient` o `radial-gradient` en botones, inputs, cards, headers o fondos de sección.
- Esquemas de color "trending" azul-violeta/morado/índigo y gradientes neon-SaaS.
- Blobs, orbs de luz, mesh gradients o fondos abstractos difusos.
- `background-clip: text` con gradientes en headings.
- Sombras dramáticas (`box-shadow` con blur > 40px o spread positivo grande).

**Patrón válido:**
- Botones: `background: var(--color-primary)` sólido. Hover: misma base con oklch ajustando luminosidad ±0.04.
- Fondos de sección: `var(--color-bg)` o `var(--color-surface)` planos, según `data-theme`.
- Si necesitas variación visual: cambia la superficie (`--color-surface` → `--color-surface-elevated`), no añadas gradiente.

**Excepciones permitidas:**
- Escalas de data viz (heatmaps, choropleths, sparklines con relleno) usando una rampa definida en tokens.
- Skeleton shimmer (gradiente animado en placeholders de carga).
- Fade overlays funcionales (ej: indicar scroll horizontal con fade en bordes de tabla).

### Regla 1.2 — Layout asimétrico en grupos de features

**Prohibido:**
- 3 columnas idénticas tipo "ícono en círculo + título + descripción".
- Íconos dentro de círculos o cuadrados con fondo de color de acento.
- Diferenciar cards con paletas distintas (card azul, card verde, card naranja).

**Patrón válido:**
- **Layout bento 2+1**: una card grande con visual real del producto + dos cards pequeñas de texto.
- **Stack lateral**: lista vertical de features con la principal expandida y las secundarias colapsadas.
- **Narrativa en zig-zag**: secciones alternadas imagen-texto / texto-imagen.
- La jerarquía se establece por tamaño, densidad de contenido y posición, **nunca por color**.

**Ejemplo de prompt correcto:**
> "Construye una sección de features con layout bento 2+1: la card grande (col-span-2) muestra captura del módulo de visitas con un título corto encima; las dos cards pequeñas son pares título+párrafo sobre reporting y permisos. Usa `Card` variante `surface-elevated` para la grande, `surface` para las pequeñas."

### Regla 1.3 — Alineación izquierda por defecto

**Prohibido:**
- Centrar body copy, descripciones de features, contenido de cards, items de lista, contenido de tablas.
- `text-align: center` en bloques de más de una línea.

**Patrón válido:**
- Hero headline corto (≤ 8 palabras) y tagline: pueden ir centrados si la composición lo pide, pero el CTA mantiene la misma alineación.
- Formularios, listas, tablas, cards y contenido editorial: alineados a la izquierda con `max-width` controlado (típico: `65ch` para prosa, `var(--container-md)` para layouts).

### Regla 1.4 — Copy específico, no genérico

**Prohibido:**
- Frases plantilla: "Empowering your journey", "Unlock the power of...", "Your all-in-one solution", "Welcome to [nombre]", "Transform your business", "Streamline your workflow".
- Lorem ipsum o texto de relleno en cualquier draft, ni siquiera temporal.
- Headlines abstractos que no nombran el dominio del producto.

**Fórmula de headline válida:**
```
[verbo activo concreto] + [audiencia específica] + [resultado medible o diferenciador]
```

- ❌ "Optimiza tu negocio con nuestra plataforma todo-en-uno."
- ✅ "Registra visitas técnicas desde el celular sin conexión y sincroniza al volver a cobertura."

**Regla práctica:** si el headline funciona tal cual para otro producto de otra industria, está mal escrito.

---

## 2. Sistemas de diseño (tokens)

Todo lo visual sale de tokens. **Cero valores arbitrarios.** Los tokens viven en `theme.css` (o `@theme` si es Tailwind 4) y se consumen vía variables CSS o utilidades del framework.

### 2.1 Color

```css
:root[data-theme="light"] {
  --color-bg: oklch(0.99 0 0);
  --color-surface: oklch(0.97 0 0);
  --color-surface-elevated: oklch(1 0 0);
  --color-text: oklch(0.2 0 0);
  --color-text-muted: oklch(0.45 0 0);
  --color-border: oklch(0.2 0 0 / 0.12);

  --color-primary: oklch(0.55 0.18 250);
  --color-primary-hover: oklch(0.51 0.18 250);
  --color-primary-active: oklch(0.47 0.18 250);
  --color-primary-soft: oklch(0.95 0.04 250);

  --color-success: oklch(0.65 0.15 145);
  --color-warning: oklch(0.75 0.15 75);
  --color-danger: oklch(0.6 0.2 25);
}
```

**Reglas:**
- Un solo color de acento principal. Variantes (hover, active, soft) se derivan ajustando `l` en oklch.
- Máximo 2 colores no-neutrales visibles en cualquier viewport.
- Colores adicionales solo en data visualization o estados semánticos (success/warning/danger).

**Dark mode:**
- Las superficies en dark suben en luminosidad cuando se elevan (bg < surface < surface-elevated), **no bajan**.
- Las sombras se reemplazan por bordes sutiles (`oklch(1 0 0 / 0.08)`).
- Los acentos pierden saturación (chroma baja ~15%) para no quemar en OLED.
- Tokens separados por modo, mismo nombre.

### 2.2 Tipografía

```css
--font-sans: "Satoshi", system-ui, sans-serif;
--font-display: "Boska", "Satoshi", serif;

--text-xs: 0.75rem;    /* 12px — meta, labels */
--text-sm: 0.875rem;   /* 14px — botones, nav */
--text-base: 1rem;     /* 16px — body */
--text-lg: 1.125rem;   /* 18px — lead paragraphs */
--text-xl: 1.5rem;     /* 24px — h3 */
--text-2xl: 2rem;      /* 32px — h2 */
--text-3xl: clamp(2.5rem, 5vw, 3.5rem);  /* h1 / hero */
```

**Reglas:**
- body mínimo 16px. Labels/meta mínimo 12px. Nunca bajar de ahí.
- `--font-display` solo desde `--text-xl` (24px) hacia arriba.
- Máximo 2 familias tipográficas. Máximo 5 combinaciones distintas de size + weight + line-height por página.
- **Fuentes prohibidas como principal:** Inter, Roboto, Open Sans, Montserrat, Poppins, Nunito.
- **Preferidas (Fontshare u otros):** Satoshi, General Sans, Cabinet Grotesk, Boska, Zodiak, Switzer.

### 2.3 Spacing

```css
--space-1: 0.25rem;    /* 4px */
--space-2: 0.5rem;     /* 8px */
--space-3: 0.75rem;    /* 12px */
--space-4: 1rem;       /* 16px */
--space-6: 1.5rem;     /* 24px */
--space-8: 2rem;       /* 32px */
--space-12: 3rem;      /* 48px */
--space-16: 4rem;      /* 64px */
--space-20: 5rem;      /* 80px */
--space-24: 6rem;      /* 96px */
```

**Reglas:**
- Todo spacing referencia tokens. Cero valores arbitrarios en px sin token (13px, 22px están prohibidos).
- Gaps entre elementos: dentro de una card `--space-3`, entre cards `--space-6`, entre secciones `--space-16` a `--space-24`.

### 2.4 Border-radius

```css
--radius-sm: 0.25rem;   /* 4px — badges, chips */
--radius-md: 0.5rem;    /* 8px — inputs, buttons */
--radius-lg: 0.75rem;   /* 12px — cards pequeñas */
--radius-xl: 1rem;      /* 16px — cards grandes, modales */
--radius-2xl: 1.5rem;   /* 24px — paneles, hero containers */
--radius-full: 9999px;  /* pills, avatares circulares */
```

**Reglas:**
- Elementos pequeños = radio pequeño, elementos grandes = radio mayor. Nunca el mismo radio en botón y en card contenedora.
- **Radio anidado:** `inner-radius = outer-radius - padding`. Evita el efecto "burbuja dentro de burbuja".
- Nunca hardcodear `border-radius: 9999px` indiscriminadamente.

### 2.5 Sombras y elevación

```css
--shadow-1: 0 1px 2px oklch(0.2 0 0 / 0.06);
--shadow-2: 0 4px 12px oklch(0.2 0 0 / 0.08);
--shadow-3: 0 12px 32px oklch(0.2 0 0 / 0.12);
```

- Light mode: 3 niveles máximo, sutiles.
- Dark mode: reemplazar sombras por bordes (`border: 1px solid oklch(1 0 0 / 0.08)`).
- Hover en cards interactivas: subir un nivel de sombra (`--shadow-1` → `--shadow-2`), **no** `transform: scale(1.05)`.

### 2.6 Ritmo de secciones (marketing)

```css
.section-dense    { padding-block: var(--space-12); }
.section-default  { padding-block: var(--space-20); }
.section-spacious { padding-block: var(--space-24); }
```

- Alternar tipos a lo largo de la página. **Prohibido** usar dos `section-default` consecutivas.
- Variar también la estructura del grid sección por sección (2 col, 3 col, sidebar, full-bleed).

---

## 3. UI de producto (dashboards, tablas, formularios)

### 3.1 Tablas

- Sin zebra striping por default. Activar con `--color-surface` muy sutil en filas pares si la tabla supera ~12 filas.
- Hover por fila: cambio a `--color-surface-elevated`, `cursor: pointer` solo si la fila es clicable.
- Headers sticky cuando la tabla scrollea (`position: sticky; top: 0`).
- Densidad como variante: compact (`--space-2` vertical), comfortable (`--space-3`, default), spacious (`--space-4`).
- **Alineación:** texto a la izquierda, números a la derecha, fechas a la izquierda con formato consistente. Encabezados siguen la alineación de su columna.
- Acciones de fila: agrupadas a la derecha en una sola columna con íconos del mismo set.

**Prohibido:** bordes gruesos entre filas; color de fondo distinto por estado de fila (usar badge en columna de estado); centrar contenido textual.

### 3.2 Filtros y toolbars

- Una sola toolbar arriba de la tabla, alineada a la izquierda. Search a la izquierda, filtros activos como chips removibles después, acciones (export, crear) a la derecha.
- Chips de filtro activo: `var(--color-primary-soft)` de fondo, label legible, X para cerrar. Máximo 4-5 visibles, el resto colapsa en "+ N filtros".
- Filtros complejos (rangos, multi-select): en panel lateral o popover, no acumulados en la toolbar.

### 3.3 StatCards / KPIs

**Estructura obligatoria (en este orden):**
1. Label pequeño en `--text-xs`, muted
2. Valor grande en `--text-2xl` o más, `--font-display` si > 24px
3. Delta opcional: ↑ 12% vs mes anterior, con color semántico

**Prohibido:** valor pequeño y label grande (jerarquía invertida); íconos decorativos grandes al lado del valor; cards con fondo de color por categoría; más de 4 KPIs en una fila.

### 3.4 Formularios

- Labels arriba del input, alineados a la izquierda, en `--text-sm`. No labels flotantes.
- Helper text bajo el input en `--text-xs` muted. Error text en el mismo lugar con `--color-danger`.
- Inputs altura mínima 40px (44px touch target en mobile).
- Agrupar campos relacionados con `--space-6` entre grupos, `--space-3` entre campos del mismo grupo.
- Botón primario abajo a la derecha en desktop, full-width en mobile.
- Validación: al hacer blur del campo, no en cada keystroke.

### 3.5 Estados defensivos

**Loading:**
- Skeleton shimmer con estructura del contenido real.
- Cargas por sección, no overlay global de página.
- Spinner centrado solo para acciones de < 2s en botones.

**Empty state:**
- Estructura: ilustración o ícono neutro pequeño + mensaje específico + acción primaria.
- "Aún no has registrado visitas este mes" > "No hay datos".
- Empty state de tabla: dentro del `<tbody>`, ocupando una fila con `colspan` completo.

**Error:**
- Mensaje específico: qué falló y qué puede hacer el usuario. Nunca códigos crudos ni stack traces.
- Ejemplo: "No pudimos conectar con el servidor de visitas. Revisa tu conexión y reintenta." + botón "Reintentar".
- Errores de campo: inline bajo el input. Errores de operación: toast o banner contextual.

---

## 4. Páginas de marketing

### 4.1 Estructura

- **Prohibido** el layout plantilla AI: hero → 3 features idénticas → 3 cards → testimonio → pricing → CTA.
- Cada página tiene un objetivo principal (signup, demo, descarga). El layout se diseña alrededor de ese objetivo.
- Navegación principal: máximo 5-7 items agrupados por caso de uso, no por módulos técnicos.

### 4.2 Hero

- Headline siguiendo la fórmula de Regla 1.4.
- Subhead opcional, máximo 2 líneas.
- Un CTA primario claro + uno secundario discreto (link, no botón secundario fuerte).
- Visual: captura real del producto, no ilustración stock.

### 4.3 Pricing

- **Tabla**, no 3 cards centradas. Las cards centradas con "Most popular" en el medio son el patrón template más obvio.
- Diferenciar tiers por contenido (qué incluye), no por color de fondo.
- Precio grande, label de periodo pequeño al lado, no debajo.

---

## 5. Imágenes, ilustraciones e iconos

- **Prohibido:** ilustraciones stock tipo undraw, 3D blobs corporativos, isométricos genéricos.
- **Preferido:** capturas reales del producto, diagramas propios, shapes geométricas neutras (líneas, grids, formas SVG simples).
- **Íconos:** un solo set por proyecto. Outline o filled, no mezclados. Mismo grosor de línea (1.5px o 2px). Tamaño consistente: 16px en meta, 20px en botones, 24px en headers.
- Todas las imágenes con `aspect-ratio` definido en CSS para evitar CLS.
- `loading="lazy"` en imágenes below-the-fold. Hero y above-the-fold sin lazy.

---

## 6. Micro-interacciones y accesibilidad

### 6.1 Animaciones

- Duración corta: 150-200ms. Más largo se siente lento.
- Easing: `cubic-bezier(0.2, 0, 0, 1)` para entradas, `cubic-bezier(0.4, 0, 1, 1)` para salidas. Nunca `linear` ni `ease`.
- Solo cuando aportan claridad: expandir panel, feedback de botón, transición de estado.
- Obligatorio respetar `prefers-reduced-motion`.

### 6.2 Estados interactivos

Todos los elementos clicables tienen 4 estados visualmente distintos:
1. `default`
2. `hover` (cambio sutil de color o sombra)
3. `active` (presionado, color más oscuro)
4. `focus-visible` (anillo de 2px con `--color-primary` y `outline-offset: 2px`)

**Prohibido:** quitar el outline sin reemplazo. `outline: none` solo si hay `:focus-visible` con anillo equivalente.

### 6.3 Accesibilidad

- Touch targets: mínimo 44×44px en mobile.
- Contraste WCAG AA: 4.5:1 en body, 3:1 en texto grande (≥ 18px).
- `alt` en todas las imágenes; vacío (`alt=""`) si es decorativa.
- `aria-label` en botones de solo ícono.
- Navegación completa por teclado: Tab order lógico, Escape cierra modales/popovers, flechas en menús y listas seleccionables.
- Anuncios de cambios dinámicos con `aria-live` para toasts y errores.

---

## 7. Performance

- Fuentes con `font-display: swap` + preload de la principal.
- Imágenes con `width` y `height` declarados o `aspect-ratio` CSS para evitar CLS.
- Code split por ruta. Componentes pesados (charts, editores) con `React.lazy`.
- Skeleton shimmer durante carga inicial de datos.
- Tablas grandes (>100 filas): virtualización (`@tanstack/react-virtual` o similar).

---

## 8. Checklist de auto-auditoría

**Al terminar cualquier UI generada, la IA corre este checklist y reporta resultado. Cualquier "sí" en una pregunta de prohibición es falla.**

### 8.1 Críticas (cualquier falla = rehacer)

- [ ] ¿Hay algún `linear-gradient` o `radial-gradient` fuera de data viz o skeleton?
- [ ] ¿Hay íconos dentro de círculos/cuadrados con color de acento?
- [ ] ¿Hay 3 columnas idénticas de features con la misma estructura?
- [ ] ¿Hay body copy centrado fuera del hero?
- [ ] ¿El headline funciona tal cual para otro producto de otra industria?
- [ ] ¿Hay `border-left` de color de acento como decoración de card?
- [ ] ¿Hay texto Lorem ipsum o placeholder genérico?

### 8.2 Sistémicas (revisar y corregir)

- [ ] ¿Todos los valores de spacing, color, radius vienen de tokens?
- [ ] ¿Hay valores en px sin token (13px, 22px)?
- [ ] ¿Se usan más de 2 familias tipográficas o más de 5 estilos distintos?
- [ ] ¿Cards e inputs comparten el mismo border-radius?
- [ ] ¿Las superficies en dark mode bajan en luminosidad al elevarse (debe ser al revés)?
- [ ] ¿Hay dos secciones consecutivas con el mismo padding-block?

### 8.3 Producto (si aplica)

- [ ] ¿Las columnas numéricas de la tabla están alineadas a la derecha?
- [ ] ¿Los StatCards tienen label arriba, valor grande abajo?
- [ ] ¿Los empty states tienen mensaje específico al contexto y acción primaria?
- [ ] ¿Los errores muestran qué hacer, no códigos crudos?

### 8.4 Pulido

- [ ] ¿Todos los elementos interactivos tienen 4 estados (default/hover/active/focus-visible)?
- [ ] ¿Las animaciones respetan `prefers-reduced-motion`?
- [ ] ¿Las imágenes tienen `aspect-ratio` o dimensiones declaradas?
- [ ] ¿Los touch targets en mobile son ≥ 44×44px?
