---
title: Estandar JavaScript — Reglas de redaccion
draft: true
---

# Estándar de Notas — JavaScript

Contrato de redacción para esta bóveda. Inspirado en el estándar de librerías del vault de Python
(API = grafo de conocimiento) y especializado al dominio del **lenguaje JavaScript**. Léelo antes de
redactar cualquier nota.

---

## 🎯 Objetivo

Documentar JavaScript como una **API y un modelo de ejecución** navegables: cada método, operador o
mecanismo es un nodo consultable aislado. Escalable, navegable, consistente y de **referencia para
relectura**, no un curso desde cero.

---

## 🧠 Filosofía base

> JS no es solo sintaxis: es un **modelo de ejecución** (scope, prototipos, event loop). Lo que más
> se consulta: *qué retorna un método, si muta, y cómo se comporta `this`*.

- **Conceptos → gobiernan** (scope, prototipos, event loop, asincronía).
- **Métodos/funciones → implementan**.
- **Los valores → estructura base** (primitivos vs objetos, referencia vs valor).

Orden por valor de relectura: arriba la **firma + el snippet que más se copia + la salida**; abajo,
cómo funciona por dentro, casos límite y relaciones.

---

## 📁 Tipos de notas

| Tipo | Qué documenta | Ejemplo de archivo |
|------|---------------|--------------------|
| Método | método de objeto/prototipo | `02 map.md` |
| Función / API | función global o Web API | `01 setTimeout.md` |
| Operador / sintaxis | operador o construcción | `07 Nullish Coalescing (??).md` |
| Concepto | mecanismo del lenguaje | `05 Orden de Ejecución (micro > macro).md` |
| Madre (`index`) | mapa de sección | `05 Promesas/index.md` |

---

## 🧩 Naming técnico (archivos)

- Nombres fijados por `_private/Tree JavaScript.md`: `NN nombre.md`, prefijo numérico por carpeta.
- Métodos/funciones **tal cual se invocan**, en minúscula: `02 map.md`, `07 Promise.all.md`,
  `04 getBoundingClientRect.md`. Coinciden EXACTAMENTE con el wikilink.
- Acentos solo en notas de concepto (`02 Composición de Funciones.md`).

---

## 🏷️ Sistema de Tags

> Máximo 3–5 tags por nota.

```yaml
tags:
  - javascript
  - api/<tipo>          # api/metodo | api/funcion | api/operador | api/concepto | api/web
  - <dominio>           # funcional | dom | async | eventos | oop | red | almacenamiento
```

- **`javascript`** obligatorio. **Regla clave:** si está en el path, no repetir en tags.
- 🚫 No usar como tag: `js`, `programacion`, `web`.

---

## 🧠 Frontmatter (estructura estándar)

```yaml
---
title: Array.prototype.map — Transformar cada elemento
aliases:
  - map
  - Array.map
tags:
  - javascript
  - api/metodo
  - funcional/arrays
# --- Clasificación ---
objeto: Array              # objeto/prototipo dueño (o "global", "Web API")
tipo: metodo               # metodo | funcion | operador | concepto | clase
# --- Comportamiento ---
retorna: Array
muta: false                # ¿muta el receptor / tiene efectos secundarios?
asincrono: false
draft: false
---
```

## 🔧 Definición de campos

- **`title`** — firma (o nombre) + `—` + descripción breve.
- **`aliases`** — nombre corto de búsqueda (`map`) + forma calificada (`Array.map`).
- **`objeto`** — prototipo/namespace dueño (`Array`, `Object`, `Promise`, `global`, `Web API`).
- **`tipo`** — `metodo | funcion | operador | concepto | clase`.
- **`retorna`** — tipo del valor de retorno. (`undefined` si no retorna).
- **`muta`** — `true`/`false`: si modifica el receptor o tiene efectos secundarios. **Clave** para
  el bloque de inmutabilidad.
- **`asincrono`** — `true` si retorna promesa / agenda trabajo.

---

## 🧱 Estructura interna por capas

**Nota de método/función (hoja):**
1. `[!definicion]` — qué hace, en una frase. Incluir la **firma**: `arr.map(callback)`.
2. **Snippet ejecutable mínimo** (```` ```js ````) **con su salida** en comentario o ```` ```text ````.
3. Parámetros y valor de retorno (tabla si hay varios).
4. `retorna` / `muta` / `asincrono` señalados explícitamente.
5. Cómo funciona por dentro / casos de uso reales (uno realista).
6. `[!warning]` trampas (`this` perdido, mutación inesperada, `==` vs `===`, callback hell).
7. `## Notas relacionadas`.

**Nota madre (`index.md`):** definición marco → modelo mental (`mermaid` para event loop, cadena de
prototipos, fases de evento) → wikilinks a las hojas → tabla comparativa → contexto.

---

## 💬 Callouts permitidos

Núcleo: `[!definicion]` `[!ejemplo]` `[!info]` `[!tip]` `[!warning]` `[!nota]`. Extensiones: `[!regla]`
`[!referencia]`.

- `[!ejemplo]` snippet + **salida real** · `[!info]` cómo funciona por dentro · `[!warning]` el bug
  clásico (TDZ, mutación, `NaN`, coerción).

> **Regla:** si quitar el callout no cambia nada, estaba mal usado.

---

## 🔗 Wikilinks y delegación

- `[[archivo | Texto]]`; carpetas con `/index`: `[[05 Promesas/index | Promesas]]`.
- **Frecuencia 1–2 por nota**, primera mención. ❌ headers, código, frontmatter, tablas.
- **Delegación:** los mecanismos viven en su sección (event loop en `07 Asíncrona`, prototipos en
  `02 POO`); el resto enlaza. El dibujo en `canvas` lo desarrolla `[[05 Canvas API/index]]`.
- **Sección final obligatoria** `## Notas relacionadas`.

---

## ✍️ Estilo de redacción

- Preferir: *el método retorna*, *la promesa se resuelve*, *el callback se invoca por cada*.
- Evitar: *recordemos*, *veamos*, *simplemente*.
- Código **ejecutable**, en ```` ```js ````, **comentado en lo no obvio**, y **siempre con su salida**
  (comentario `// → ...` o bloque ```` ```text ````). Mostrar el HTML mínimo si la nota toca el DOM.

---

## 🎨 Convenciones del dominio (JavaScript)

> [!info] Estándar de referencia
> **ECMAScript (TC39)** y **Web APIs (WHATWG/W3C)**, ejecutado en navegador moderno. Lo de Node se
> indica como tal. Lo legacy (`var`, XHR) se marca como legado.

- Nombres reales entre `code`: `Array.prototype.map`, `addEventListener`, `Promise.all`.
- Señalar siempre **retorno y mutación** (lo más consultado para programación funcional).
- Para asincronía y DOM, dar el ejemplo que hace **visible** el mecanismo (orden de logs, timestamps).
- Diagramas (event loop, prototype chain, fases de evento) en ```` ```mermaid ````.

---

## ✅ Test de aceptación

- ¿Frontmatter con `title — descripción`, alias de búsqueda, tags `javascript` + `api/<tipo>` + dominio?
- ¿Firma + snippet ejecutable **con salida** arriba? ¿`retorna`/`muta` explícitos?
- ¿Trampas cubiertas con `[!warning]`? ¿Callouts que aportan?
- ¿Wikilinks 1–2 con `/index`, y `## Notas relacionadas` al final?

Responder SOLO con el Markdown de la nota.
