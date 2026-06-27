---
title: let — Declaración de variable con ámbito de bloque
aliases:
  - let
  - declaración let
tags:
  - javascript
  - api/concepto
  - variables
objeto: global
tipo: concepto
draft: false
order: 2
---

# let

> [!definicion]
> `let` declara una variable con **ámbito de bloque**: el identificador solo existe dentro del bloque `{}` que lo contiene. Se eleva al inicio del bloque (*hoisting*) pero no se inicializa — permanece en la **Temporal Dead Zone** hasta la línea de declaración. No es re-declarable en el mismo ámbito y no se adjunta a `window`.

```js
let contador = 0;
contador++;
console.log(contador); // 1

{
  let local = "solo aquí";
}
console.log(local); // ReferenceError: local is not defined
```

## Ámbito de bloque

Cualquier par de llaves `{}` crea un nuevo ámbito para `let`: bloques `if`, `for`, `while`, `try/catch`, y bloques desnudos.

```js
if (true) {
  let resultado = 42;
  console.log(resultado); // 42
}
console.log(resultado); // ReferenceError

for (let i = 0; i < 3; i++) {
  console.log(i); // 0, 1, 2
}
console.log(i); // ReferenceError
```

Comparado con `var`, la variable no "se filtra" fuera del bloque.

## Hoisting y TDZ

`let` se eleva al inicio del bloque pero el motor la marca como *uninitialized*. El acceso antes de la declaración lanza `ReferenceError`, no devuelve `undefined` como haría `var`.

```js
{
  console.log(x); // ReferenceError: Cannot access 'x' before initialization
  let x = 5;
  console.log(x); // 5
}
```

El período entre el inicio del bloque y la línea de declaración es la Temporal Dead Zone. Ver [[05 Temporal Dead Zone]].

## No re-declarable en el mismo ámbito

```js
let nombre = "Ana";
let nombre = "Luis"; // SyntaxError: Identifier 'nombre' has already been declared
```

Sí es posible declarar el mismo nombre en un bloque anidado (shadowing):

```js
let valor = 1;
{
  let valor = 2; // shadowing: nueva binding, no reasignación
  console.log(valor); // 2
}
console.log(valor); // 1
```

## No se adjunta a `window`

```js
let config = { debug: true };
console.log(window.config); // undefined
```

Esto evita la contaminación del objeto global que produce `var`.

## El bug clásico de `var` en bucles

Con `var`, todos los callbacks de un bucle comparten el mismo binding de `i` — al ejecutarse los timeouts, `i` ya vale el valor final.

```js
// Con var — bug:
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// → 3, 3, 3

// Con let — correcto:
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// → 0, 1, 2
```

`let` crea un binding **nuevo por cada iteración** del bucle. El motor JS genera implícitamente un bloque de ámbito separado por vuelta, por lo que cada closure captura su propia copia de `i`.

## Casos de uso ideales

- **Contadores** en bucles (`let i = 0`).
- **Acumuladores** que cambian a lo largo de un cálculo.
- **Variables de control de flujo** (`let encontrado = false`).
- **Reasignación necesaria**: resultado de un `switch`, valor que se actualiza en varios pasos.

Cuando no se necesita reasignar, `const` es preferible. Ver [[03 const]].

## Cómo funciona por dentro

Cada bloque `{}` crea un **Lexical Environment** nuevo con su propio *Environment Record*. Al entrar al bloque, el motor registra los identificadores `let`/`const` como *bindings sin inicializar*. La instrucción de inicialización en el código fuente es la que lleva el valor al registro. Los accesos a bindings sin inicializar están prohibidos a nivel de especificación (la operación `GetBindingValue` lanza `ReferenceError` si el binding está en estado *uninitialized*).

Para los bucles `for`, el motor crea un entorno de iteración por cada vuelta y copia el valor actual de `i` al entorno de la siguiente iteración.

> [!warning] Errores comunes
> **TDZ en bloques condicionales:** declarar `let` dentro de un `if` y usarla fuera produce `ReferenceError`, no `undefined`. Este error es más descriptivo que el silencioso `undefined` de `var`, pero requiere conocer la TDZ para interpretarlo.
> ```js
> if (condicion) {
>   let temp = calcular();
> }
> console.log(temp); // ReferenceError
> ```
> **Shadowing no intencional:** un `let` en un bloque anidado con el mismo nombre que una variable externa oculta la externa sin advertencia del runtime.

> [!tip] Buenas prácticas
> Usar `let` solo cuando la reasignación sea necesaria. En cualquier otro caso, `const` comunica mejor la intención. Declarar la variable en el bloque más estrecho posible para minimizar su exposición.

## Notas relacionadas

- [[01 var]]
- [[03 const]]
- [[04 Hoisting]]
- [[05 Temporal Dead Zone]]
- [[06 Ámbito (scope)/index|Ámbito (scope)]]
