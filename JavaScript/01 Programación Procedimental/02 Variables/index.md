---
title: Variables — Declaración, ámbito y ciclo de vida
aliases:
  - Variables JS
  - var let const
tags:
  - javascript
  - api/concepto
  - variables
draft: false
order: 2
---

# Variables

> [!definicion]
> Una variable es un enlace nombrado (*binding*) entre un identificador y un valor almacenado en memoria. En JavaScript, ese enlace se crea con `var`, `let` o `const`; la elección determina el **ámbito**, el comportamiento ante el **hoisting** y la **capacidad de reasignación**.

```js
var  nombre = "Ana";   // función-scope, hoisting con undefined
let  edad   = 30;      // bloque-scope, TDZ hasta la declaración
const PI    = 3.14159; // bloque-scope, binding inmutable
```

## Tabla comparativa

| Característica | `var` | `let` | `const` |
|---|---|---|---|
| Ámbito | función | bloque | bloque |
| Hoisting | sí, inicia como `undefined` | sí, pero en TDZ | sí, pero en TDZ |
| Re-declarable en mismo ámbito | sí | no (`SyntaxError`) | no (`SyntaxError`) |
| Re-asignable | sí | sí | no (`TypeError`) |
| Se adjunta a `window` (global) | sí | no | no |
| Requiere inicialización | no | no | sí |

## Las tres formas de declarar

### `var` — legado y ámbito de función

`var` declara en el ámbito de la **función envolvente** (o en el global si está en el nivel superior). Los bloques `if`, `for`, `while` no crean un ámbito nuevo para `var`. Su declaración se eleva (*hoisting*) al inicio de la función, inicializada a `undefined`. Ver [[01 var]].

### `let` — bloque-scope, reasignable

`let` confina la variable al bloque `{}` más cercano. Se eleva al inicio del bloque pero permanece en la **Temporal Dead Zone** hasta la línea de declaración: cualquier acceso previo lanza `ReferenceError`. Ver [[02 let]].

### `const` — bloque-scope, binding inmutable

`const` exige inicialización en la declaración y prohíbe la **reasignación del binding** (no congela el contenido del objeto o array). Es la opción por defecto en código moderno; se cambia a `let` solo cuando la reasignación es necesaria. Ver [[03 const]].

## Hoisting y Temporal Dead Zone

El motor JS procesa todas las declaraciones antes de ejecutar el código. Con `var` el resultado es que la variable existe desde el inicio de la función (valor `undefined`). Con `let`/`const` el motor conoce la variable pero la deja *sin inicializar* — ese período es la [[05 Temporal Dead Zone]]. Ver [[04 Hoisting]] para el mecanismo completo.

## Ámbito (scope)

El ámbito determina desde qué partes del código se puede leer o escribir una variable. JavaScript soporta cuatro tipos: global, función, bloque y léxico. El ámbito léxico es la base de los closures: una función recuerda el ámbito donde fue **definida**, no donde fue llamada. Ver [[06 Ámbito (scope)/index|Ámbito (scope)]].

## Buenas prácticas de nombrado

Los identificadores siguen convenciones por tipo: `camelCase` para variables y funciones, `PascalCase` para clases, `SCREAMING_SNAKE_CASE` para constantes de valor fijo. Los nombres describen el *qué*, no el *cómo*. Ver [[07 Buenas Prácticas en Nombrado]].

> [!tip] Regla de oro
> Declarar siempre con `const`. Si la lógica exige reasignar, usar `let`. Usar `var` solo al mantener código legado.

## Notas relacionadas

- [[01 var]]
- [[02 let]]
- [[03 const]]
- [[04 Hoisting]]
- [[05 Temporal Dead Zone]]
- [[06 Ámbito (scope)/index|Ámbito (scope)]]
- [[07 Buenas Prácticas en Nombrado]]
