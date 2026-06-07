---
title: null — Ausencia intencional de objeto
aliases:
  - null JS
  - tipo null
tags:
  - javascript
  - api/concepto
  - tipos
objeto: global
tipo: concepto
draft: false
---

# null

> [!definicion]
> `null` es un valor primitivo que representa la **ausencia intencional de un objeto**. A diferencia de `undefined` (que el motor asigna automáticamente), `null` lo asigna el programador para comunicar explícitamente que una variable que debería apuntar a un objeto no apunta a ninguno todavía (o ha sido vaciada).

```js
let usuario = null;  // el usuario no está cargado aún
console.log(usuario); // → null
```

## El bug de `typeof null`

```js
typeof null   // → "object"
```

`typeof null === "object"` es un **bug histórico de JavaScript** (presente desde 1995) que no se puede corregir sin romper compatibilidad hacia atrás. La representación interna original usaba una etiqueta de 3 bits donde `000` indicaba "objeto", y el puntero nulo también tenía bits en cero, causando la clasificación errónea.

Para comprobar si algo es efectivamente `null`, la única forma fiable es la comparación estricta:

```js
let val = null;
val === null      // → true   (correcto)
typeof val        // → "object"  (no es suficiente para detectar null)
```

## Comparación con `undefined`

| Aspecto | `null` | `undefined` |
|---|---|---|
| Asignado por | El programador | El motor JS |
| Semántica | Ausencia intencional de objeto | No inicializado / no existe |
| `typeof` | `"object"` | `"undefined"` |
| `== undefined` | `true` | `true` |
| `=== undefined` | `false` | `true` |
| Truthy/Falsy | falsy | falsy |

```js
null == undefined    // → true   (doble igual: ambos son nullish)
null === undefined   // → false  (triple igual: tipos distintos)
```

## Valores nullish

`null` y `undefined` forman la categoría de valores **nullish**: son los únicos dos valores que el operador `??` (nullish coalescing) trata como "ausentes".

```js
// ?? usa el operando derecho solo si el izquierdo es null o undefined
null ?? "por defecto"       // → "por defecto"
undefined ?? "por defecto"  // → "por defecto"
0 ?? "por defecto"          // → 0   (0 no es nullish)
"" ?? "por defecto"         // → ""  ("" no es nullish)
false ?? "por defecto"      // → false  (false no es nullish)
```

Lo mismo aplica al optional chaining `?.`: cortocircuita si el receptor es `null` o `undefined`.

```js
const usuario = null;
usuario?.nombre    // → undefined  (no lanza TypeError)
usuario.nombre     // → TypeError: Cannot read properties of null
```

## Cuándo usar `null`

El patrón convencional es asignar `null` a una variable cuando:

- Debería contener un objeto, pero aún no está disponible (carga asíncrona, resultado pendiente).
- El resultado de una búsqueda o consulta fue "no encontrado" (retornar `null` es más explícito que `undefined`).
- Se quiere vaciar una referencia a objeto para permitir que el garbage collector lo libere.

```js
let cache = null;             // sin datos inicialmente

function cargarCache() {
  cache = { datos: [] };
}

function limpiarCache() {
  cache = null;               // liberar referencia
}

function buscar(id) {
  return encontrado ? resultado : null;  // no encontrado → null explícito
}
```

> [!tip] Buenas prácticas
> - Usar `null` para indicar "este slot debería tener un objeto pero está vacío"; reservar `undefined` para "ausencia de definición".
> - Para comprobar si algo es `null` o `undefined` a la vez, usar `valor == null` (doble igual); es el patrón más conciso.
> - Preferir `?.` sobre comprobaciones manuales `if (obj !== null && obj !== undefined)`.

> [!warning] Errores comunes
> - `typeof null === "object"` no indica que `null` sea un objeto; es un bug. No usar `typeof` para detectar `null`.
> - `null.propiedad` lanza `TypeError`; nunca acceder a propiedades de `null` sin comprobar antes (con `?.` o `if`).
> - `null + 1` → `1` (coerción: `null` se convierte a `0` en contexto numérico). Comportamiento sorprendente en operaciones aritméticas mixtas.

## Notas relacionadas

- [[04 undefined|undefined]] — diferencia semántica con `null`
- [[04 Conversión de Tipos/03 Truthy y Falsy|Truthy y Falsy]] — `null` es falsy
- [[04 Conversión de Tipos/04 Igualdad (== vs ===)|Igualdad == vs ===]] — `null == undefined`
- [[01 Primitivos/index|Primitivos]]
