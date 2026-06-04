---
title: Estandar HTML — Reglas de redaccion
draft: true
---

# Estándar de Notas — HTML

Contrato de redacción para esta bóveda. Inspirado en el estándar de librerías del vault de Python
(API = grafo de conocimiento) y especializado al dominio del **lenguaje de marcado HTML**. Léelo
antes de redactar cualquier nota.

---

## 🎯 Objetivo

Documentar HTML como una **API navegable**: cada elemento/atributo es un nodo consultable de forma
aislada. El sistema debe ser:

- **Escalable** — cientos de notas sin degradación.
- **Navegable** — grafo limpio, delegación por wikilink.
- **Consistente** — misma anatomía en toda nota.
- **De referencia** — para relectura rápida, NO un libro introductorio.

---

## 🧠 Filosofía base

> HTML no es una lista de etiquetas: es un **árbol semántico**. La decisión *qué elemento usar* es
> lo que más se consulta.

- **Conceptos → gobiernan** (semántica, accesibilidad, categorías de contenido).
- **Elementos → implementan** (la etiqueta concreta y sus atributos).
- **El documento → estructura base** (DOCTYPE, `head`, `body`).

Organización por **valor de relectura**: lo más copiado arriba (snippet de marcado, tabla de
atributos); abajo, accesibilidad, casos límite y relaciones. Prohibido `Introducción`, `Motivación`,
`Panorama`.

---

## 📁 Tipos de notas

| Tipo | Qué documenta | Ejemplo de archivo |
|------|---------------|--------------------|
| Elemento | una etiqueta concreta | `01 Enlaces (a).md` |
| Concepto | idea transversal | `01 HTML Semántico como Base.md` |
| Madre (`index`) | mapa de una sección/carpeta | `07 Formularios/index.md` |

---

## 🧩 Naming técnico (archivos)

- Los nombres ya están fijados por `_private/Tree HTML.md`: `NN Nombre Descriptivo (etiqueta).md`,
  con prefijo numérico de orden por carpeta. **Coinciden EXACTAMENTE** con el wikilink.
- La etiqueta real va entre paréntesis al final: `04 Énfasis Fuerte (strong).md`.
- Acentos permitidos en el nombre (estilo Python). El número ordena dentro de la carpeta.

---

## 🏷️ Sistema de Tags

> Máximo 3–5 tags por nota.

```yaml
tags:
  - html
  - api/<tipo>          # api/elemento | api/atributo | api/concepto
  - <dominio>           # semantica | formularios | multimedia | metadatos | a11y | tablas
```

- **`html`** obligatorio. Añadir **`a11y`** si la nota carga peso de accesibilidad.
- **Regla clave:** si ya está en el path (la carpeta), **no repetir** en tags.
- 🚫 No usar como tag: `web`, `etiqueta`, `teoria`.

---

## 🧠 Frontmatter (estructura estándar)

```yaml
---
title: <a> — Enlaces de hipertexto
aliases:
  - anchor
  - enlace
tags:
  - html
  - api/elemento
  - semantica/navegacion
# --- Clasificación ---
elemento: a
categoria: fraseo          # flujo | seccionado | fraseo | metadatos | incrustado | interactivo
# --- Comportamiento ---
rol_implicito: link        # rol ARIA implícito (o "ninguno")
vacio: false               # elemento void (sin etiqueta de cierre)
draft: false
---
```

## 🔧 Definición de campos

- **`title`** — `<etiqueta>` (o nombre) + `—` + descripción breve. Es la interfaz humana.
- **`aliases`** — nombre real en inglés del elemento (`anchor`, `image`) + sinónimos de búsqueda.
- **`elemento`** — la etiqueta sin `<>`. Omitir en notas de concepto.
- **`categoria`** — categoría de contenido del estándar (gobierna dónde puede anidar).
- **`rol_implicito`** — rol ARIA que el navegador asigna por defecto.
- **`vacio`** — `true` para elementos void (`img`, `br`, `input`…).

---

## 🧱 Estructura interna por capas

**Nota de elemento (hoja):**
1. `[!definicion]` — qué es, en una frase precisa.
2. **Snippet de marcado mínimo** renderizable (```` ```html ````).
3. **Tabla de atributos** (`atributo · valores · descripción`) si los tiene.
4. Semántica y comportamiento por defecto (cuándo usarlo, qué NO hace).
5. Ejemplo realista.
6. `[!info]` accesibilidad: rol implícito, teclado, alternativa accesible.
7. `[!warning]` errores comunes (anidamiento inválido, atributos obsoletos).
8. `## Notas relacionadas`.

**Nota madre (`index.md`):** definición marco → mapa (puede ir `mermaid`) → wikilinks a las hojas
con una línea cada una → tabla comparativa → contexto.

---

## 💬 Callouts permitidos

Núcleo: `[!definicion]` `[!ejemplo]` `[!info]` `[!tip]` `[!warning]` `[!nota]`. Extensiones con
mesura: `[!regla]` `[!referencia]`.

- `[!definicion]` qué es · `[!ejemplo]` marcado + render · `[!info]` semántica/rol implícito ·
  `[!tip]` buenas prácticas · `[!warning]` riesgos (XSS en `innerHTML`, `target="_blank"` sin
  `rel="noopener"`).

> **Regla:** si quitar el callout no cambia nada, estaba mal usado.

---

## 🔗 Wikilinks y delegación

- Formato `[[archivo | Texto visible]]`. **Carpetas SIEMPRE con `/index`**:
  `[[01 Imágenes/index | Imágenes]]`.
- **Frecuencia: 1–2 por nota**, en la primera mención significativa. No saturar.
- ✔️ En párrafos y listas. ❌ En headers, código, frontmatter, tablas.
- **Delegación:** si un concepto tiene nota propia, NO se desarrolla aquí; se enlaza. Lo dinámico va
  a JS (`[[05 dataset]]`, `[[05 Canvas API/index]]`); lo visual, a CSS.
- **Sección final obligatoria** `## Notas relacionadas` con las notas citadas o las más afines.

---

## ✍️ Estilo de redacción

- Preferir: *el elemento representa*, *el navegador renderiza por defecto*, *se anida dentro de*.
- Evitar (suena a tutor): *recordemos*, *veamos*, *como podrás imaginar*.
- Marcado **tal cual se escribe**, en ```` ```html ````, 2 espacios de indentación, minúsculas.

---

## 🎨 Convenciones del dominio (HTML)

> [!info] Estándar de referencia
> **HTML Living Standard (WHATWG)** + HTML5; navegadores modernos. Lo obsoleto se marca como tal.

- Nombres reales entre `code`: `<a>`, `srcset`, `aria-label`. Texto en español.
- Mencionar **rol ARIA implícito** y **categoría de contenido** cuando importe.
- Separar estructura (HTML) de presentación (CSS) y comportamiento (JS): nada de `style`/`onclick`
  inline salvo para ilustrar la mala práctica.
- Diagramas de árbol DOM o flujo de formulario en ```` ```mermaid ````.

---

## ✅ Test de aceptación

- ¿Frontmatter con `title — descripción`, alias en inglés, tags `html` + `api/<tipo>` + dominio?
- ¿Snippet de marcado y tabla de atributos arriba; ejemplo temprano?
- ¿Semántica + accesibilidad cubiertas? ¿Callouts que aportan?
- ¿Wikilinks 1–2, con `/index` en carpetas, y `## Notas relacionadas` al final?

Responder SOLO con el Markdown de la nota.
