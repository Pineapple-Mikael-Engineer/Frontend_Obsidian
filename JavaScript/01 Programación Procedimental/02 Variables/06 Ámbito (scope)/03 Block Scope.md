---
title: Block Scope — El ámbito de bloque en JavaScript
aliases:
  - block scope
  - ámbito de bloque
tags:
  - javascript
  - api/concepto
  - variables
tipo: concepto
draft: false
order: 3
---

# Block Scope

> [!definicion]
> El **ámbito de bloque** (*block scope*) es el ámbito creado por cualquier par de llaves `{}`. Las variables `let` y `const` declaradas dentro son accesibles únicamente dentro de ese bloque. `var` ignora los bloques y sube al ámbito de la función envolvente.

```js
{
  let x = 10;
  const Y = 20;
  var z = 30; // sube a la función o al global
}

console.log(x); // ReferenceError
console.log(Y); // ReferenceError
console.log(z); // 30  ← var se filtra del bloque
```

## Tipos de bloques en JavaScript

Todo bloque `{}` crea ámbito para `let`/`const`:

```js
// if / else:
if (true) {
  let soloEnIf = "aquí";
}
console.log(soloEnIf); // ReferenceError

// for:
for (let i = 0; i < 3; i++) { /* … */ }
console.log(i); // ReferenceError

// while:
while (condicion) {
  let iter = calcular();
  // iter solo existe en esta iteración
}

// try / catch:
try {
  JSON.parse("{mal");
} catch (err) {
  console.log(err.message); // accesible
}
console.log(err); // ReferenceError — err siempre tuvo ámbito de bloque

// Bloque desnudo:
{
  let temporal = prepararDatos();
  procesarDatos(temporal);
} // temporal se libera aquí
```

## El bug clásico de `var` en bucles `for`

Con `var`, todos los callbacks comparten la misma binding de `i` porque `var` sube al ámbito de la función:

```js
const funciones = [];

for (var i = 0; i < 3; i++) {
  funciones.push(() => console.log(i));
}

funciones[0](); // 3  ← no 0
funciones[1](); // 3
funciones[2](); // 3
// i vale 3 al terminar el bucle; todos los callbacks ven el mismo i
```

Con `let`, el motor crea un nuevo binding por cada iteración:

```js
const funciones = [];

for (let i = 0; i < 3; i++) {
  funciones.push(() => console.log(i));
}

funciones[0](); // 0
funciones[1](); // 1
funciones[2](); // 2
```

Internamente, el motor genera un bloque de ámbito separado por vuelta y copia el valor de `i` de una iteración a la siguiente, por lo que cada closure captura su propia copia.

## `catch` siempre tuvo ámbito de bloque

El parámetro `err` de un bloque `catch` ha tenido ámbito de bloque desde ES3, incluso antes de que existiera block scope para `let`/`const`. Esto es una excepción histórica al comportamiento de `var`.

```js
try {
  undefined();
} catch (error) {
  console.log(error); // accesible
}
console.log(error); // ReferenceError en todos los motores (siempre fue así)
```

Desde ES2019, el parámetro de `catch` es opcional si no se usa:

```js
try {
  operacionArriesgada();
} catch {
  console.log("falló");
}
```

## Bloques desnudos para limitar el ámbito

Se puede crear un bloque sin ninguna construcción de control para limitar la visibilidad de variables temporales:

```js
function procesarPedido(pedido) {
  const total = calcularTotal(pedido);

  {
    const descuento = aplicarCupon(pedido.cupon);
    const totalConDescuento = total - descuento;
    registrar(totalConDescuento);
  }
  // descuento y totalConDescuento no existen aquí

  return total;
}
```

Este patrón ayuda al lector a identificar qué variables son temporales de un paso intermedio.

## Cómo funciona por dentro

Cada vez que el motor entra en un bloque `{}`, crea un nuevo **Lexical Environment** cuyo *Declarative Environment Record* contiene los bindings `let`/`const` del bloque. El campo `[[OuterEnv]]` apunta al entorno del bloque o función que lo contiene. Al salir del bloque, el entorno se descarta (salvo referencias desde closures).

`var` no genera bindings en el entorno del bloque: el motor la registra en el *Function Environment Record* durante la fase de creación del contexto de ejecución, antes de que el código comience a ejecutarse.

> [!warning] Errores comunes
> **`var` en bucles con callbacks asíncronos:** el bug de `var` en `for` con `setTimeout`, `fetch`, o handlers de eventos es uno de los más frecuentes en código legado. Siempre usar `let` en bucles que capturan la variable de iteración en un callback.
> **Asumir que `switch` crea múltiples bloques:** un `switch` es un solo bloque `{}`. Declarar `let` con el mismo nombre en dos `case` distintos lanza `SyntaxError` porque están en el mismo bloque. Usar bloques explícitos por `case` si se necesita.
> ```js
> switch (x) {
>   case 1: {
>     let resultado = "uno";
>     break;
>   }
>   case 2: {
>     let resultado = "dos"; // OK — bloque propio
>     break;
>   }
> }
> ```

> [!tip] Buenas prácticas
> Siempre `let`/`const` en bucles, nunca `var`. Declarar las variables en el bloque más estrecho donde se usan: reduce el riesgo de shadowing, libera memoria antes y comunica la intención al lector. Usar bloques desnudos para agrupar lógica con variables temporales en funciones largas.

## Notas relacionadas

- [[index|Ámbito (scope)]]
- [[01 Global Scope]]
- [[02 Function Scope]]
- [[04 Lexical Scope]]
- [[../01 var|var]]
- [[../02 let|let]]
