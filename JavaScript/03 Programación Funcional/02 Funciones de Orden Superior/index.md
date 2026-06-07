---
title: Funciones de Orden Superior
aliases:
  - HOF
  - higher-order functions
  - funciones de orden superior
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
---

# Funciones de Orden Superior

> [!definicion]
> Una **función de orden superior (HOF, _higher-order function_)** es una función que cumple al menos una de estas condiciones: recibe una o más funciones como argumentos, o devuelve una función como resultado. Son el mecanismo central de la programación funcional en JavaScript porque permiten **abstraer el comportamiento** — separar el _qué_ se hace del _cómo_ se hace.

```js
// HOF que recibe función
[1, 2, 3].map(x => x * 2);           // [2, 4, 6]

// HOF que devuelve función
function crearSaludo(saludo) {
  return nombre => `${saludo}, ${nombre}!`;
}
const hola = crearSaludo("Hola");
hola("Mundo");                        // "Hola, Mundo!"
```

## Los dos enfoques fundamentales

| Enfoque | Descripción | Ejemplos nativos |
|---|---|---|
| Recibir funciones | Acepta un callback y lo invoca en el momento adecuado | `map`, `filter`, `reduce`, `forEach`, `setTimeout`, `addEventListener` |
| Retornar funciones | Devuelve un closure con estado o configuración capturada | Factories, memoize, curry, decoradores, partial application |

## Por qué importan las HOFs

Las funciones de orden superior son el puente entre código imperativo y funcional. En lugar de escribir bucles `for` para cada transformación, se expresa la **intención** directamente:

```js
// Imperativo: el cómo oscurece el qué
const dobles = [];
for (let i = 0; i < nums.length; i++) {
  dobles.push(nums[i] * 2);
}

// Funcional con HOF: el qué es evidente
const dobles = nums.map(x => x * 2);
```

La ganancia no es solo sintáctica. Al extraer el comportamiento variable (la función `x => x * 2`) del mecanismo de iteración (`map`), se obtiene:

- **Reutilización**: el mismo `map` sirve para cualquier transformación.
- **Composabilidad**: las HOFs se encadenan y combinan fácilmente.
- **Testeabilidad**: cada función pura se prueba de forma aislada.

## HOFs nativas de JavaScript

Las APIs del lenguaje están construidas sobre este patrón. Los métodos de `Array` más usados:

| Método | Qué hace | Firma del callback |
|---|---|---|
| `map(fn)` | Transforma cada elemento; devuelve nuevo array | `(elem, idx, arr) => nuevoElem` |
| `filter(fn)` | Filtra por predicado; devuelve nuevo array | `(elem, idx, arr) => boolean` |
| `reduce(fn, init)` | Acumula a un único valor | `(acum, elem, idx, arr) => nuevoAcum` |
| `forEach(fn)` | Ejecuta efecto por elemento; devuelve `undefined` | `(elem, idx, arr) => void` |
| `find(fn)` | Primer elemento que cumple predicado | `(elem, idx, arr) => boolean` |
| `some(fn)` | `true` si algún elemento cumple | `(elem, idx, arr) => boolean` |
| `every(fn)` | `true` si todos cumplen | `(elem, idx, arr) => boolean` |
| `sort(fn)` | Ordena in-place según comparador | `(a, b) => número` |
| `flatMap(fn)` | Mapea y aplana un nivel | `(elem, idx, arr) => elem | elem[]` |

Fuera de `Array`, también son HOFs: `setTimeout(fn, ms)`, `addEventListener(evento, fn)`, `Promise.then(fn)`, `Array.from({ length }, fn)`.

## Abstracción de comportamiento

El poder de las HOFs reside en separar la **estructura** del **comportamiento**:

```js
// La estructura (iterar y acumular) está en reduce
// El comportamiento (sumar) viene como argumento
const suma  = arr => arr.reduce((ac, x) => ac + x, 0);
const max   = arr => arr.reduce((ac, x) => x > ac ? x : ac, -Infinity);
const unión = arr => arr.reduce((ac, x) => [...ac, ...x], []);
```

El mismo mecanismo de `reduce` sirve para tres operaciones completamente distintas porque el comportamiento se pasa desde afuera.

> [!tip] Buenas prácticas
> - Mantén los callbacks cortos y con un único propósito; si el callback necesita más de dos líneas, considera extraerlo como función nombrada.
> - Prefiere los métodos de `Array` nativos sobre bucles manuales cuando la intención sea una transformación estándar.
> - Las HOFs que devuelven funciones deben documentar qué capturan en el closure para evitar sorpresas al inspeccionar el estado.

> [!warning] Errores comunes
> - Confundir **invocar** el callback con **pasarlo**: `arr.map(fn())` invoca `fn` y pasa su resultado (probablemente `undefined`); lo correcto es `arr.map(fn)`.
> - Usar `forEach` cuando se necesita el array transformado — `forEach` siempre devuelve `undefined`; usa `map`.
> - Olvidar el valor inicial en `reduce` sobre arrays que pueden estar vacíos: sin valor inicial, `reduce` lanza `TypeError` en array vacío.

## Notas relacionadas

- [[01 Recibir Funciones (callbacks)]]
- [[02 Retornar Funciones]]
