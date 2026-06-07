---
title: var — Declaración de variable con ámbito de función
aliases:
  - var
  - declaración var
tags:
  - javascript
  - api/concepto
  - variables
objeto: global
tipo: concepto
draft: false
---

# var

> [!definicion]
> `var` declara una variable con **ámbito de función** (o global si está en el nivel superior del script). Su declaración se eleva al inicio de la función —*hoisting*— inicializada a `undefined`. Es re-declarable en el mismo ámbito sin error y, en el ámbito global, crea una propiedad en `window`.

```js
function ejemplo() {
  console.log(x); // undefined  ← hoisting
  var x = 10;
  console.log(x); // 10
}
ejemplo();
```

## Ámbito de función, no de bloque

`var` ignora los bloques `if`, `for`, `while` y `{}` desnudos. La variable queda visible en toda la función envolvente.

```js
function sinBloque() {
  if (true) {
    var mensaje = "hola";
  }
  console.log(mensaje); // "hola"  ← se filtró del bloque
}

function conBucle() {
  for (var i = 0; i < 3; i++) { /* … */ }
  console.log(i); // 3  ← i sigue viva tras el for
}
```

Con `let` o `const` ambos ejemplos lanzarían `ReferenceError` fuera del bloque.

## Hoisting — declaración elevada, inicialización no

El motor JS eleva la **declaración** al inicio de la función antes de ejecutar ninguna línea. La **inicialización** permanece en su lugar original. El resultado es que la variable existe desde el comienzo con valor `undefined`.

```js
// Código escrito:
function f() {
  console.log(a); // ¿qué imprime?
  var a = 5;
  console.log(a);
}

// Lo que el motor "ve" internamente:
function f() {
  var a;            // declaración elevada → undefined
  console.log(a);   // undefined
  a = 5;            // inicialización en su sitio
  console.log(a);   // 5
}
```

## Re-declaración sin error

Re-declarar la misma variable con `var` en el mismo ámbito no produce ningún error; la segunda declaración actúa como una re-asignación silenciosa.

```js
var color = "rojo";
var color = "azul"; // sin error, color ahora es "azul"
console.log(color); // "azul"
```

Con `let` la misma operación lanza `SyntaxError: Identifier 'color' has already been declared`.

## `var` en el ámbito global y `window`

Una variable `var` declarada en el nivel superior de un script (fuera de cualquier función) se convierte en propiedad del objeto `window` en el navegador.

```js
var appVersion = "1.0";
console.log(window.appVersion); // "1.0"

let build = 42;
console.log(window.build); // undefined  ← let no se adjunta a window
```

Esto puede causar colisiones con propiedades existentes de `window` (p. ej. `window.name`, `window.status`).

## Cómo funciona por dentro

Cuando el motor entra en una función, crea un **Variable Environment** (un *Environment Record* de tipo *Declarative*). Durante la fase de *creación del contexto de ejecución*, el parser recorre el cuerpo de la función, detecta todas las declaraciones `var` y añade cada identificador a ese registro con valor inicial `undefined`. Solo cuando la ejecución llega a la línea de asignación se escribe el valor real.

En el ámbito global, el Variable Environment está respaldado por el **objeto global** (`window`/`global`), por lo que cada `var` global se materializa como propiedad enumerable de ese objeto.

## Por qué evitarla en código moderno

- El ámbito de función genera variables que "se filtran" fuera de los bloques, lo que produce bugs difíciles de rastrear.
- La re-declaración silenciosa oculta errores tipográficos.
- La adjunción a `window` contamina el ámbito global y puede colisionar con APIs del navegador.
- El hoisting con `undefined` hace que el código sea menos predecible para quien lo lee.

`let` y `const` (ES6) resuelven los tres problemas con ámbito de bloque y TDZ.

## Cuándo sigue apareciendo

- **Código legado**: cualquier base de código anterior a 2015 usa `var` de forma exclusiva.
- **Scripts sin transpilador** ejecutados directamente en IE11 o entornos muy antiguos.
- **Tutoriales y libros viejos**: conviene reconocerla para entender el código que se encuentra, aunque no debe usarse en código nuevo.

> [!warning] Errores comunes
> **Bug clásico de closures en bucles:** con `var`, todos los callbacks del bucle comparten la misma variable `i` (que al ejecutarse vale el valor final del bucle).
> ```js
> for (var i = 0; i < 3; i++) {
>   setTimeout(() => console.log(i), 100);
> }
> // Imprime: 3, 3, 3  (no 0, 1, 2)
> ```
> La solución moderna es `let`. Ver [[02 let#El bug clásico de `var` en bucles]].

> [!tip] Buenas prácticas
> No usar `var` en código nuevo. Preferir `const` por defecto y `let` cuando se necesite reasignar. Reservar el conocimiento de `var` para leer y mantener código legado.

## Notas relacionadas

- [[02 let]]
- [[03 const]]
- [[04 Hoisting]]
- [[05 Temporal Dead Zone]]
- [[06 Ámbito (scope)/index|Ámbito (scope)]]
