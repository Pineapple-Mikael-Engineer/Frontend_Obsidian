---
title: Declaraciones y Expresiones — La distinción fundamental de JS
aliases:
  - expressions vs statements
  - expresiones JS
  - declaraciones JS
  - expression statement
tags:
  - javascript
  - api/concepto
  - sintaxis
objeto: global
tipo: concepto
draft: false
---

# Declaraciones y Expresiones

> [!definicion]
> Una **expresión** (*expression*) es cualquier fragmento de código que el motor JS evalúa para producir un valor. Una **declaración** (*statement*) es una instrucción completa que hace algo — ejecuta lógica, define estructura — pero no produce un valor usable directamente. Esta distinción determina qué puede aparecer en cada posición del código: los sitios que esperan una expresión (después de `=`, dentro de `()`, como argumento) no aceptan declaraciones.

```js
// EXPRESIONES — cada una produce un valor
42                    // literal numérico → 42
"hola"                // literal string  → "hola"
true                  // literal boolean → true
1 + 2                 // operador binario → 3
x > 0 ? "pos" : "neg" // operador ternario → string
Math.random()         // llamada a función → number [0,1)
(x) => x * 2          // arrow function   → Function
[1, 2, 3]             // literal array    → Array
{ nombre: "Ana" }     // literal objeto   → Object (en contexto de expresión)

// DECLARACIONES — instrucciones, no producen valor usable
var x;                // declara variable
if (x > 0) { }       // condicional
for (let i = 0; i < 3; i++) { }  // bucle
function doble(n) { return n * 2; }  // declaración de función
class Punto { }       // declaración de clase
```

## Expresiones en detalle

### Expresiones literales

Los literales son la forma más básica de expresión: representan un valor directamente escrito en el código.

```js
100          // number
3.14         // number (float)
"texto"      // string
`plantilla`  // template literal (también es expresión)
true         // boolean
null         // null
undefined    // undefined (identificador global, no palabra clave)
/patrón/gi   // RegExp literal
```

### Expresiones con operadores

Los operadores combinan subexpresiones para producir un nuevo valor. La precedencia y asociatividad determinan el orden de evaluación.

```js
5 * (3 + 2)          // → 25  (agrupación fuerza orden)
"Hola" + " " + "Ana" // → "Hola Ana"  (concatenación)
typeof "abc"         // → "string"  (operador unario prefijo)
arr.length           // → number  (acceso a propiedad — también es expresión)
obj?.propiedad       // → valor o undefined  (optional chaining)
a ?? b               // → a si no es null/undefined, sino b
```

### Expresiones de llamada a función

Una llamada a función es siempre una expresión, porque produce el valor de retorno (`undefined` si la función no retorna explícitamente).

```js
Math.max(3, 7)       // → 7
[1, 2, 3].map(x => x * 2)  // → [2, 4, 6]
console.log("x")     // → undefined  (el efecto es la impresión; el valor es undefined)
```

### Expresiones de función (Function Expressions)

Una función puede aparecer como expresión cuando se asigna a una variable, se pasa como argumento, o se llama inmediatamente.

```js
// Asignada a variable
const cuadrado = function(n) { return n * n; };
const triple   = (n) => n * 3;   // arrow function expression

// Como argumento (callback)
setTimeout(function() { console.log("ok"); }, 1000);
[1, 2, 3].forEach(n => console.log(n));

// Inmediatamente invocada (IIFE)
(function() { console.log("autoejecutada"); })();
```

> [!info]
> Una **arrow function** es siempre una expresión: no existe "declaración de arrow function". `(x) => x * 2` produce un valor de tipo `Function`; por eso se asigna, se pasa o se llama, pero no se "declara" en el sentido de `function foo() {}`.

## Declaraciones en detalle

### Declaraciones de variable

```js
var  nombre = "Ana";   // legado — hoisting, scope de función
let  edad   = 30;      // bloque, TDZ, reasignable
const MAX   = 100;     // bloque, TDZ, no reasignable (el valor puede mutar si es objeto)
```

### Declaraciones de control de flujo

```js
if (x > 0) {
  console.log("positivo");
} else {
  console.log("negativo o cero");
}

for (let i = 0; i < 5; i++) {
  console.log(i);
}

while (condicion) { /* … */ }
switch (valor) { case 1: break; }
```

### Declaración de función (`function declaration`)

```js
function suma(a, b) {
  return a + b;
}
```

La diferencia clave con una function expression: la declaración de función experimenta **hoisting completo** — el nombre y el cuerpo quedan disponibles desde el inicio del scope, antes de la línea donde aparece. Ver [[02 Variables/04 Hoisting | Hoisting]].

### Declaración de clase

```js
class Rectangulo {
  constructor(w, h) { this.w = w; this.h = h; }
  area() { return this.w * this.h; }
}
```

Las clases también se hoisteen (el nombre existe en el TDZ) pero no inicializan antes de la línea. Las clases son siempre estrictas.

## Expression Statements

Una **expression statement** es una expresión usada como declaración: se evalúa, produce un valor, y ese valor se descarta. Es la forma más frecuente de sentencia en código real.

```js
// Expression statements típicos:
console.log("mensaje");    // la expresión es la llamada; el valor (undefined) se descarta
x++;                       // incremento — efecto secundario sobre x
array.push(42);            // muta array; el valor de retorno (nueva longitud) se descarta
fetch("/api/data");        // lanza la petición; la promesa retornada se descarta (¡cuidado!)
```

> [!warning]
> Descartar la promesa de `fetch()` u otras funciones async sin `.catch()` provoca que los errores pasen silenciosamente. Si no se necesita el valor de retorno, al menos encadenar `.catch(console.error)`.

## El contexto importa: `{}` como bloque vs objeto

El parser JS decide si `{}` es un **bloque de código** (declaración) o un **literal de objeto** (expresión) por el contexto en que aparece.

```js
// Bloque (inicio de sentencia): el parser lo interpreta como bloque vacío
{}              // bloque vacío — válido, no hace nada
{ x: 1 }       // también bloque: `x: 1` es una labeled statement (raro pero válido)

// Objeto (contexto de expresión): después de `=`, en argumento, etc.
const obj = { x: 1 };   // aquí sí es un objeto literal
([{ x: 1 }]);            // dentro de `()` → expresión → objeto
```

Esto explica por qué una arrow function que retorna un objeto literal necesita paréntesis:

```js
const getObj = () => ({ id: 1, nombre: "Ana" });
// Sin los (): () => { id: 1 } se interpreta como bloque con labeled statement
const getObjMal = () => { id: 1 };  // ¡retorna undefined, no el objeto!
```

## Posiciones que esperan expresión vs declaración

```js
// Después de `=` → expresión obligatoria
const n = if (true) { 1 } // ❌ SyntaxError: if es declaración

// Argumento de función → expresión obligatoria
console.log(for (;;) {})  // ❌ SyntaxError

// Después de `return` → expresión
function f() {
  return 1 + 2;  // ✓ expresión
}

// Condición de if/while → expresión
if (x = 5) { }   // ✓ sintácticamente válido (asignación es expresión), pero ⚠️ probablemente bug
```

> [!tip] Buenas prácticas
> - Preferir **function declarations** para funciones que definen comportamiento principal del módulo — el hoisting completo permite leer el código de arriba hacia abajo sin preocuparse por el orden de definición.
> - Preferir **function expressions** (arrow o clásicas) para callbacks, funciones de orden superior y funciones que dependen de un `this` léxico.
> - Cuando una arrow function retorna un objeto literal, siempre envolver en `()` para evitar la ambigüedad con bloques.
> - No descartar valores de retorno de funciones async sin gestionar el error.

> [!warning] Errores comunes
> - Intentar usar una declaración (`if`, `for`, `class`) donde se espera una expresión genera `SyntaxError` inmediato.
> - `const obj = () => { clave: valor }` retorna `undefined` porque `{}` es un bloque, no un objeto.
> - `var x = function foo() {}` — `foo` solo existe dentro de la función, no en el scope externo (a diferencia de una function declaration).
> - Confundir `console.log(x = 5)` (asignación-expresión, imprime 5) con `console.log(x == 5)` (comparación).

## Notas relacionadas

- [[02 Punto y Coma]]
- [[04 Modo Estricto (use strict)]]
- [[02 Variables/04 Hoisting | Hoisting]]
- [[02 Variables/05 Temporal Dead Zone | Temporal Dead Zone]]
- [[03 Programación Funcional/03 Arrow Functions/index | Arrow Functions]]
- [[07 Funciones (básico)/01 Declaración de Función | Declaración de Función]]
- [[07 Funciones (básico)/02 Expresión de Función | Expresión de Función]]
