---
title: Function.prototype.call() — Invocar con this explícito
aliases:
  - call
  - Function.prototype.call
  - fn.call
tags:
  - javascript
  - api/metodo
  - this
objeto: Function
tipo: metodo
retorna: any
muta: false
asincrono: false
draft: false
order: 7
---

# `Function.prototype.call()`

> [!definicion]
> `fn.call(thisArg, arg1, arg2, …)` invoca `fn` de forma inmediata con `this = thisArg` y argumentos individuales. Establece **explicit binding** — prioridad de binding solo superada por `new`.

```js
function saludar(saludo, puntuacion) {
  return `${saludo}, ${this.nombre}${puntuacion}`;
}

const usuario = { nombre: "Ana" };

saludar.call(usuario, "Hola", "!");
// → "Hola, Ana!"
```

## Firma

```
fn.call(thisArg[, arg1[, arg2[, …]]])
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `thisArg` | any | Valor de `this` dentro de `fn` |
| `arg1, arg2, …` | any | Argumentos pasados a `fn` como individuales |
| **Retorna** | any | El valor devuelto por `fn` |

## `thisArg` nulo o undefined

| Modo | `thisArg` | `this` dentro de `fn` |
|---|---|---|
| No estricto | `null` o `undefined` | Objeto global (`window`) |
| No estricto | primitivo (`"texto"`, `42`) | Wrapper correspondiente (`String`, `Number`) |
| Estricto | `null` | `null` |
| Estricto | `undefined` | `undefined` |
| Estricto | primitivo | el primitivo sin envolver |

```js
function mostrar() { "use strict"; console.log(this); }

mostrar.call(null);      // → null
mostrar.call(42);        // → 42  (primitivo, sin wrapper)
mostrar.call("texto");   // → "texto"
```

## Receta: tomar prestado un método de otro objeto

```js
const array = [3, 1, 4, 1, 5, 9];
const max = Math.max.call(null, ...array);
// → 9  — null porque Math.max no usa this

const nodoLista = document.querySelectorAll("li");
const arr = Array.prototype.slice.call(nodoLista);
// → Array real — idiomático pre-ES6 (hoy: Array.from(nodoLista))
```

## Receta: detectar tipo real de un valor

`typeof` no distingue objetos complejos. `Object.prototype.toString.call(val)` devuelve la etiqueta interna `[[Class]]`.

```js
Object.prototype.toString.call([]);           // → "[object Array]"
Object.prototype.toString.call(null);         // → "[object Null]"
Object.prototype.toString.call(new Date());   // → "[object Date]"
Object.prototype.toString.call(/rx/);         // → "[object RegExp]"
Object.prototype.toString.call(Promise.resolve()); // → "[object Promise]"
```

## Receta: herencia prototípica clásica — llamar al constructor padre

```js
function Animal(nombre, sonido) {
  this.nombre = nombre;
  this.sonido = sonido;
}

function Perro(nombre) {
  Animal.call(this, nombre, "Guau"); // inyecta propiedades de Animal en this de Perro
  this.patas = 4;
}

Perro.prototype = Object.create(Animal.prototype);
Perro.prototype.constructor = Perro;

const rex = new Perro("Rex");
console.log(rex.nombre);  // → "Rex"
console.log(rex.sonido);  // → "Guau"
```

## Cómo funciona por dentro

> [!info]
> `call` es el mecanismo primitivo de invocación con contexto explícito. Internamente crea un nuevo **Execution Context** para `fn` donde `ThisBinding = thisArg` (con las conversiones de wrapper/global según modo estricto). Los argumentos se pasan directamente como la lista de argumentos formal de `fn`. Es equivalente a `fn.apply(thisArg, [arg1, arg2, …])` — la diferencia entre `call` y `apply` es únicamente la forma de proporcionar los argumentos.

## Buenas prácticas

> [!tip]
> Preferir `call` sobre `apply` cuando el número de argumentos es conocido en tiempo de escritura — es más legible. Usar `null` como `thisArg` cuando la función no usa `this` (métodos matemáticos, utilidades puras), señalando explícitamente que el contexto es irrelevante.

## Errores comunes

> [!warning]
> En modo no estricto, pasar `null` como `thisArg` establece `this = window`, lo que puede producir contaminación del global si la función asigna propiedades a `this`. En modo estricto esto es seguro. Verificar que el código de la función esté en modo estricto antes de asumir que `call(null, …)` es inocuo.

## Notas relacionadas

- [[index|this — Contexto de invocación]] — prioridades de binding
- [[08 apply()]] — identical a `call` pero con array de argumentos
- [[09 bind()]] — versión que devuelve función en lugar de invocar
- [[06 Pérdida de this]] — cuándo usar `call` para recuperar el contexto
