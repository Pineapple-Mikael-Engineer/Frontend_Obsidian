---
title: Declaración de Función — Hoisting completo
aliases:
  - function declaration
  - declaración de función
tags:
  - javascript
  - api/concepto
  - funciones
objeto: global
tipo: concepto
draft: false
order: 1
---

# Declaración de Función

> [!definicion]
> Una **declaración de función** define una función con nombre usando la palabra clave `function` al inicio de una instrucción. Su rasgo distintivo es el **hoisting completo**: el motor registra tanto el nombre como el cuerpo durante la fase de parsing, antes de ejecutar ninguna línea de código.

```js
// Llamada ANTES de la definición — funciona por hoisting
const resultado = cuadrado(5); // → 25

function cuadrado(n) {
  return n * n;
}
```

## Sintaxis

```js
function nombre(param1, param2, ...paramN) {
  // cuerpo
  return valor;
}
```

El nombre es obligatorio en una declaración (a diferencia de la expresión de función, donde es opcional).

## Hoisting completo

Durante la fase de parsing (antes de la ejecución), el motor recorre el scope en busca de declaraciones de función y las registra íntegramente en el ámbito. No hay Temporal Dead Zone: la función está disponible desde la primera línea del scope.

```js
// El motor procesa esto en dos fases:
// FASE 1 (parsing): registra doble() con su cuerpo completo en el scope
// FASE 2 (ejecución): ejecuta línea a línea

console.log(typeof doble); // → "function" (ya existe)
doble(3);                  // → 6

function doble(x) {
  return x * 2;
}
```

Contrasta con `var` (hoisting solo del nombre, valor `undefined`) y `let`/`const` (TDZ hasta la declaración). Ver [[JavaScript/01 Programación Procedimental/02 Variables/04 Hoisting | Hoisting]].

## Propiedades de la función

Toda función creada con una declaración es un objeto `Function` con propiedades inspeccionables:

```js
function registrar(nombre, edad, ciudad) {
  return `${nombre}, ${edad}, ${ciudad}`;
}

registrar.name;   // → "registrar"
registrar.length; // → 3  (parámetros formales)
typeof registrar; // → "function"
```

`fn.length` cuenta solo los parámetros **sin valor por defecto** y **sin rest**: si el tercer parámetro fuera `ciudad = "Madrid"`, `length` devolvería `2`.

## Recursión

La función puede referenciarse a sí misma por su nombre desde el cuerpo, sin necesidad de ninguna variable externa:

```js
function factorial(n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
}

factorial(5); // → 120
```

La referencia al nombre `factorial` se resuelve en el mismo scope donde se declaró; el motor la encuentra por hoisting.

## Propiedad en el objeto global

En scripts no-módulo ejecutados en el scope global (no con `"use strict"` a nivel módulo), una declaración de función crea una propiedad en `globalThis`:

```js
// En un script de navegador (no module)
function saluda() { return "hola"; }

globalThis.saluda; // → [Function: saluda]
window.saluda();   // → "hola"
```

En módulos ES6 o con `"use strict"` en función, esto no ocurre: la función queda en el ámbito del módulo, no en `globalThis`.

## Declaraciones dentro de bloques

Las declaraciones de función dentro de bloques `if`, `for`, etc. tienen comportamiento definido por el modo:

```js
// Modo estricto: la función existe solo dentro del bloque
"use strict";
if (true) {
  function hola() { return "hola"; }
  hola(); // OK
}
// hola(); // ReferenceError fuera del bloque
```

En modo no-estricto el comportamiento varía por entorno y no es confiable. La práctica correcta es usar expresiones de función para funciones condicionales.

## Recetas comunes

**Función de utilidad a nivel de módulo** — el caso más habitual para declaración:

```js
// utils.js
export function formatearFecha(date) {
  return new Intl.DateTimeFormat("es-ES").format(date);
}

export function calcularIVA(precio, tasa = 0.21) {
  return precio * (1 + tasa);
}
```

**Recursión mutua** — dos funciones que se llaman entre sí; el hoisting garantiza que ambas estén disponibles cuando cualquiera de ellas se ejecuta:

```js
function esPar(n) {
  if (n === 0) return true;
  return esImpar(n - 1);
}

function esImpar(n) {
  if (n === 0) return false;
  return esPar(n - 1);
}

esImpar(7); // → true
```

## Cómo funciona por dentro

El motor de JavaScript opera en dos fases sobre cada scope (módulo, función, bloque):

1. **Parsing / creación del scope**: el motor escanea las declaraciones de función y las registra en el objeto de entorno (`VariableEnvironment`). El objeto `Function` se crea en el heap y la referencia queda almacenada en el binding del nombre. No hay TDZ.
2. **Ejecución**: cuando el control llega a la línea de la declaración, el motor la salta (ya estaba registrada). Las llamadas anteriores o posteriores a esa línea resuelven el mismo objeto.

Las expresiones de función, en cambio, crean el objeto `Function` durante la ejecución, cuando el motor alcanza esa línea.

> [!tip] Buenas prácticas
> - Usar declaraciones para funciones independientes que definan la lógica pública de un módulo o script: son más legibles y el hoisting facilita organizar el código con las funciones al final.
> - Usar expresiones de función (o arrow) para callbacks, funciones anónimas y funciones asignadas condicionalmente.
> - Evitar declaraciones de función dentro de bloques (`if`, `for`) — usar expresiones de función ahí.

> [!warning] Errores comunes
> **Confundir declaración con expresión** — olvidar que una declaración no puede ser anónima:
> ```js
> function () { return 1; } // SyntaxError: declaración requiere nombre
> const f = function () { return 1; }; // correcto: expresión de función
> ```
> **Sobrescritura silenciosa por hoisting** — dos declaraciones con el mismo nombre en el mismo scope: la segunda sobreescribe a la primera sin error, lo cual puede ser un bug difícil de detectar:
> ```js
> function procesar(x) { return x * 2; }
> function procesar(x) { return x + 100; } // sobreescribe sin error
> procesar(5); // → 105, no 10
> ```

## Notas relacionadas

- [[07 Funciones (básico)/index | Funciones (básico)]]
- [[02 Expresión de Función]]
- [[JavaScript/01 Programación Procedimental/02 Variables/04 Hoisting | Hoisting]]
- [[JavaScript/01 Programación Procedimental/02 Variables/05 Temporal Dead Zone | Temporal Dead Zone]]
- [[JavaScript/02 Programación Orientada a Objetos/02 Prototipos/05 Funciones Constructoras (new, this) | Funciones Constructoras]]
