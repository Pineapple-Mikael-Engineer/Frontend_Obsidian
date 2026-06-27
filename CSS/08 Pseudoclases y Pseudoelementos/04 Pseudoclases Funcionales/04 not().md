---
title: not() — Excluir elementos
aliases:
  - ":not"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":not"
grupo: pseudoclase
draft: false
order: 4
---

# :not()

> [!definicion]
> `:not(selector)` selecciona los elementos que **no** coinciden con el selector dado. Permite excluir casos de una regla sin tener que enumerar todo lo demás, evitando reglas largas o sobrescrituras.

```css
li:not(.activo) { opacity: 0.6; }   /* todos los items menos el activo */
```

## Excluir en vez de sobrescribir

> [!tip] Aplicar a todos menos a algunos
> En vez de aplicar un estilo a todos y luego **deshacerlo** en algunos, `:not()` lo aplica directamente solo donde toca:
> ```css
> /* ❌ aplicar y deshacer */
> button { margin-right: 1rem; }
> button:last-child { margin-right: 0; }
>
> /* ✅ excluir directamente */
> button:not(:last-child) { margin-right: 1rem; }
> ```
> Es más limpio y expresa mejor la intención ("todos menos el último").

## Lista de selectores (moderno)

> [!info] :not() acepta varios selectores
> Las versiones modernas de `:not()` aceptan una **lista** separada por comas, excluyendo todos a la vez:
> ```css
> /* Excluir varios tipos */
> input:not([type="checkbox"], [type="radio"]) { width: 100%; }
> /* = no checkboxes ni radios */
> ```
> Antes había que encadenar `:not(a):not(b)`; ahora `:not(a, b)` es más legible.

## La especificidad de :not()

> [!warning] :not() añade la especificidad de su argumento
> `:not()` **sí** aporta especificidad: la del selector de su interior (como [[01 is() | `:is()`]], la del más alto de la lista). Esto puede subir el peso de la regla:
> ```css
> :not(#x) { }   /* especificidad de #x (0,1,0,0), aunque coincida con casi todo */
> ```
> La pseudoclase `:not()` en sí no añade peso, pero su **argumento** sí. Cuidado al meter `#id` dentro.

## Casos de uso

```css
/* Bordes entre items, no en el último */
.lista > :not(:last-child) { border-bottom: 1px solid #eee; }

/* Espaciado entre elementos (lobotomized owl) */
* + * { margin-top: 1rem; }
.flow > :not(:first-child) { margin-top: 1rem; }

/* Estilar enlaces externos (no los internos) */
a:not([href^="/"]):not([href^="#"]) { /* enlaces externos */ }

/* Validación que no marca campos vacíos */
input:not(:placeholder-shown):invalid { border-color: red; }
```

## not() anidado y combinado

`:not()` se combina con otras pseudoclases para condiciones precisas:

```css
button:not(:disabled):hover { background: #b48ef0; }   /* hover solo si no está deshabilitado */
:focus:not(:focus-visible) { outline: none; }           /* quitar anillo en clics de ratón */
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `:not()` para excluir casos en vez de aplicar-y-deshacer.
> - Aprovecha la lista moderna `:not(a, b)` para excluir varios a la vez.
> - Cuidado con la especificidad del **argumento** (no metas `#id` a la ligera).
> - Combínalo con otras pseudoclases para condiciones precisas (`:not(:disabled):hover`).

## Errores comunes

> [!warning] Trampas
> - **Especificidad alta** por el argumento de `:not()`.
> - **`:not()` encadenados** innecesarios cuando la lista `:not(a, b)` es más clara.
> - **Lógica invertida confusa**: demasiados `:not()` anidados dificultan leer la intención.

## Notas relacionadas

- [[01 is()]] · [[02 where()]] — agrupar (lo contrario de excluir).
- [[03 has()]] — seleccionar por relación.
- [[01 Especificidad/index]] — cómo el argumento de `:not()` pesa.
