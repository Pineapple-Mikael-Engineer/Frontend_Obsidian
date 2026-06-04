---
title: Estandar CSS — Reglas de redaccion
draft: true
---

# Estándar de Notas — CSS

Contrato de redacción para esta bóveda. Inspirado en el estándar de librerías del vault de Python
(API = grafo de conocimiento) y especializado al dominio de **hojas de estilo CSS**. Léelo antes de
redactar cualquier nota.

---

## 🎯 Objetivo

Documentar CSS como una **API de propiedades y selectores** navegable: cada propiedad/valor/selector
es un nodo consultable aislado. Escalable, navegable (grafo limpio), consistente y de **referencia
para relectura**, no un tutorial.

---

## 🧠 Filosofía base

> CSS no se memoriza, se consulta. Lo que más se busca: *qué valores acepta una propiedad* y *cómo
> interactúa con la cascada*.

- **Conceptos → gobiernan** (cascada, especificidad, modelo de caja, contexto de formato).
- **Propiedades → implementan** (la declaración concreta y sus valores).
- **El render → resultado** (qué se ve, y por qué).

Orden por valor de relectura: arriba la **tabla de valores** y el snippet que más se copia; abajo,
interacción con la cascada, casos límite y relaciones.

---

## 📁 Tipos de notas

| Tipo | Qué documenta | Ejemplo de archivo |
|------|---------------|--------------------|
| Propiedad | una propiedad CSS | `04 font-weight.md` |
| Selector | selector / combinador / pseudo | `03 nth-child().md` |
| Función | función de valor | `06 calc().md` |
| Concepto | idea transversal | `01 Cálculo de Especificidad.md` |
| Madre (`index`) | mapa de sección | `03 Flexbox/index.md` |

---

## 🧩 Naming técnico (archivos)

- Nombres fijados por `_private/Tree CSS.md`: `NN nombre.md`, prefijo numérico por carpeta.
- **La propiedad/selector va en minúscula y tal cual** se escribe en CSS: `04 font-weight.md`,
  `03 nth-child().md`, `02 hover.md`. Coinciden EXACTAMENTE con el wikilink.
- Acentos permitidos solo en notas de concepto (`01 Cálculo de Especificidad.md`).

---

## 🏷️ Sistema de Tags

> Máximo 3–5 tags por nota.

```yaml
tags:
  - css
  - api/<tipo>          # api/propiedad | api/selector | api/funcion | api/concepto | api/pseudo
  - <dominio>           # layout | tipografia | fondos | animacion | responsive | arquitectura
```

- **`css`** obligatorio. **Regla clave:** si está en el path, no repetir en tags.
- 🚫 No usar como tag: `estilo`, `propiedad`, `web`.

---

## 🧠 Frontmatter (estructura estándar)

```yaml
---
title: flex-grow — Factor de crecimiento de un ítem flex
aliases:
  - flex-grow
tags:
  - css
  - api/propiedad
  - layout/flexbox
# --- Clasificación ---
propiedad: flex-grow
grupo: flexbox             # box-model | tipografia | flexbox | grid | fondo | animacion...
# --- Comportamiento ---
valor_inicial: "0"
hereda: false
animable: true
shorthand_de: flex         # opcional: propiedad shorthand que la engloba
draft: false
---
```

## 🔧 Definición de campos

- **`title`** — `propiedad` + `—` + descripción breve.
- **`aliases`** — el nombre exacto de la propiedad/selector (para búsqueda tipo API).
- **`propiedad`** — el identificador CSS sin valor. (En selectores usar `selector:`).
- **`valor_inicial`** — valor inicial según especificación.
- **`hereda`** — `true`/`false`: si la propiedad se hereda de forma natural.
- **`animable`** — `true`/`false`: si participa en transiciones/animaciones.
- **`shorthand_de`** — si es subpropiedad de un shorthand, cuál.

---

## 🧱 Estructura interna por capas

**Nota de propiedad (hoja):**
1. `[!definicion]` — qué controla, en una frase.
2. **Tabla de valores** (`valor · efecto`) — lo más consultado.
3. **Snippet CSS mínimo** (```` ```css ````) y, si aporta, el render esperado (descrito o capturable).
4. Valor inicial, herencia, si es animable.
5. Interacción con la cascada / box model / contexto de formato.
6. `[!warning]` gotchas (colapso de márgenes, especificidad, `!important`).
7. `## Notas relacionadas`.

**Nota madre (`index.md`):** definición marco → diagrama mental (`mermaid` si ayuda) → wikilinks a
las hojas → tabla comparativa de propiedades → contexto.

---

## 💬 Callouts permitidos

Núcleo: `[!definicion]` `[!ejemplo]` `[!info]` `[!tip]` `[!warning]` `[!nota]`. Extensiones: `[!regla]`
`[!referencia]`.

- `[!ejemplo]` snippet + resultado · `[!tip]` truco práctico (centrado, `clamp()` fluido) ·
  `[!warning]` lo que rompe el layout o la cascada.

> **Regla:** si quitar el callout no cambia nada, estaba mal usado.

---

## 🔗 Wikilinks y delegación

- `[[archivo | Texto]]`; carpetas con `/index`: `[[04 CSS Grid/index | Grid]]`.
- **Frecuencia 1–2 por nota**, primera mención. ❌ headers, código, frontmatter, tablas.
- **Delegación:** la cascada/especificidad viven en `09 Arquitectura`; el resto enlaza. La
  manipulación de variables con JS delega a `[[01 Propiedad style]]` (JS).
- **Sección final obligatoria** `## Notas relacionadas`.

---

## ✍️ Estilo de redacción

- Preferir: *la propiedad define*, *el valor calculado resulta*, *bajo el contexto de formato flex*.
- Evitar: *recordemos*, *veamos*, *fácilmente*.
- CSS **tal cual se escribe**, en ```` ```css ````. Mostrar también el HTML mínimo cuando el efecto
  dependa de la estructura.

---

## 🎨 Convenciones del dominio (CSS)

> [!info] Estándar de referencia
> Especificaciones **CSS del W3C/WHATWG** y soporte en navegadores modernos. Lo experimental o de
> bajo soporte se marca explícitamente.

- Nombres de propiedades, valores y funciones entre `code`: `display`, `auto-fit`, `minmax()`.
- Indicar siempre **valor inicial, herencia y si es animable** (lo más consultado).
- Para layout, mostrar el contenedor + ítems mínimos; describir ejes/pistas.
- Diagramas de cajas/ejes en ```` ```mermaid ```` o imágenes en `_media/` si aportan.

---

## ✅ Test de aceptación

- ¿Frontmatter con `title — descripción`, alias = nombre API, tags `css` + `api/<tipo>` + dominio?
- ¿Tabla de valores y snippet arriba; valor inicial/herencia/animable presentes?
- ¿Interacción con cascada/box model cubierta? ¿Callouts que aportan?
- ¿Wikilinks 1–2 con `/index`, y `## Notas relacionadas` al final?

Responder SOLO con el Markdown de la nota.
