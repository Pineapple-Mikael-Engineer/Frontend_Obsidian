---
title: Funciones de Primera Clase — Funciones como valores
aliases:
  - first-class functions
  - first-class values
  - funciones de primera clase
  - ciudadanos de primera clase
tags:
  - javascript
  - api/concepto
  - funciones
objeto: global
tipo: concepto
draft: false
---

# Funciones de Primera Clase

> [!definicion]
> En JavaScript, las funciones son **valores de primera clase** (*first-class values*): se almacenan en variables, se pasan como argumentos, se devuelven desde otras funciones y se guardan en estructuras de datos, de la misma forma que un número o un string. El motor las implementa como objetos del tipo `Function` en el heap.

```js
// Asignar a variable
const saludar = function(nombre) { return `Hola, ${nombre}`; };

// Pasar como argumento
[1, 2, 3].map(n => n * 2); // → [2, 4, 6]

// Devolver desde otra función
function multiplicar(factor) {
  return n => n * factor; // retorna una función
}
const triple = multiplicar(3);
triple(7); // → 21

// Guardar en una estructura de datos
const operaciones = {
  sumar: (a, b) => a + b,
  restar: (a, b) => a - b,
};
operaciones.sumar(10, 3); // → 13
```

## Funciones de orden superior (HOF)

Una **función de orden superior** recibe funciones como argumentos, devuelve funciones, o ambas. Son la base del estilo funcional en JavaScript:

```js
// HOF integrada — Array.filter recibe una función predicado
const pares = [1, 2, 3, 4, 5, 6].filter(n => n % 2 === 0);
// → [2, 4, 6]

// HOF integrada — Array.reduce con acumulador
const suma = [10, 20, 30].reduce((acc, n) => acc + n, 0);
// → 60

// HOF integrada — setTimeout recibe un callback
setTimeout(() => console.log("3 segundos después"), 3000);
```

## Callbacks

Un **callback** es una función pasada como argumento para ser invocada más tarde, ya sea de forma síncrona o asíncrona:

```js
// Callback síncrono — Array.sort
const nombres = ["Carlos", "Ana", "Beatriz"];
nombres.sort((a, b) => a.localeCompare(b));
// → ["Ana", "Beatriz", "Carlos"]

// Callback asíncrono — evento del DOM
document.querySelector("#btn").addEventListener("click", function(event) {
  console.log("Clic detectado:", event.target);
});
```

El patrón de callbacks es el fundamento de la programación asíncrona y orientada a eventos. Su evolución se desarrolla en [[JavaScript/07 Programación Asíncrona/04 Callbacks/index | Callbacks]].

## Funciones que devuelven funciones — Closures

Cuando una función devuelve otra función, la función interna **captura** el entorno léxico de la externa: puede acceder a las variables de la función padre incluso después de que esta haya retornado. Esto es un **closure**:

```js
function crearContador(inicio = 0) {
  let count = inicio; // capturado en el closure
  return {
    incrementar() { count++; },
    decrementar() { count--; },
    valor()       { return count; }
  };
}

const c = crearContador(10);
c.incrementar();
c.incrementar();
c.valor(); // → 12
// count no es accesible desde fuera — encapsulación real
```

Los closures se desarrollan en [[JavaScript/02 Programación Orientada a Objetos/05 Encapsulación y Abstracción/01 Closures para Datos Privados | Closures para Datos Privados]].

## Currificación

La **currificación** (*currying*) transforma una función de múltiples argumentos en una cadena de funciones unarias. Cada llamada fija un argumento y devuelve una función esperando el siguiente:

```js
// Función normal
const sumar = (a, b) => a + b;
sumar(3, 4); // → 7

// Versión currificada
const sumarCurried = a => b => a + b;
sumarCurried(3)(4); // → 7

// Aplicación parcial — fijar el primer argumento
const sumarDiez = sumarCurried(10);
sumarDiez(5);  // → 15
sumarDiez(20); // → 30
```

```js
// Caso real: currificar un formateador de moneda
const formatearMoneda = moneda => cantidad =>
  new Intl.NumberFormat("es-ES", { style: "currency", currency: moneda })
    .format(cantidad);

const enEuros = formatearMoneda("EUR");
const enDolares = formatearMoneda("USD");

enEuros(1234.5);   // → "1.234,50 €"
enDolares(1234.5); // → "$1,234.50"
```

El currying completo se desarrolla en [[JavaScript/03 Programación Funcional/06 Currying y Composición/01 Currying | Currying]].

## Composición de funciones

Dos funciones `f` y `g` se **componen** como `f ∘ g`: se aplica primero `g` y luego `f` sobre el resultado:

```js
const compose = (f, g) => x => f(g(x));

const duplicar = x => x * 2;
const sumarUno = x => x + 1;

const duplicarYSumarUno = compose(sumarUno, duplicar);
duplicarYSumarUno(5); // → 11  (5 * 2 = 10, 10 + 1 = 11)
```

Para componer más de dos funciones, se usa `reduceRight`:

```js
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);

const pipeline = compose(
  x => x.trim(),
  x => x.toLowerCase(),
  x => x.replace(/\s+/g, "-")
);
pipeline("  Hola Mundo  "); // → "hola-mundo"
```

La composición y `pipe` (orden inverso) se desarrollan en [[JavaScript/03 Programación Funcional/06 Currying y Composición/03 compose | compose]].

## Memoización

La memoización almacena los resultados de llamadas previas para evitar recalcularlos. Se implementa como una HOF que envuelve la función original:

```js
function memoizar(fn) {
  const cache = new Map();
  return function(...args) {
    const clave = JSON.stringify(args);
    if (cache.has(clave)) return cache.get(clave);
    const resultado = fn.apply(this, args);
    cache.set(clave, resultado);
    return resultado;
  };
}

const fibonacciLento = n => {
  if (n <= 1) return n;
  return fibonacciLento(n - 1) + fibonacciLento(n - 2);
};

const fibonacci = memoizar(fibonacciLento);
fibonacci(40); // primera vez: lento
fibonacci(40); // segunda vez: instantáneo (cache hit)
```

La memoización como patrón se desarrolla en [[JavaScript/12 Patrones y Buenas Prácticas/04 Memoización | Memoización]].

## Tabla comparativa: primera clase vs segunda clase

| Operación | JS (primera clase) | Lenguajes con funciones de segunda clase |
|---|---|---|
| Asignar a variable | sí — `const f = fn` | no (o con puntero explícito) |
| Pasar como argumento | sí — `map(fn)` | limitado o con wrapper |
| Devolver desde función | sí — `return fn` | raro o imposible |
| Guardar en array/objeto | sí — `[fn1, fn2]` | no |
| Crear anónimamente | sí — `x => x * 2` | generalmente no |

## Cómo funciona por dentro

Las funciones son objetos de tipo `Function` almacenados en el **heap**. Las variables que "contienen" funciones almacenan en realidad una referencia (dirección de memoria) a ese objeto, igual que con cualquier objeto.

Lo que habilita los closures es que el objeto `Function` lleva consigo un slot interno `[[Environment]]`, que es un enlace al **entorno léxico** donde fue creada. Cuando la función se invoca, el motor crea un nuevo entorno de ejecución cuyo entorno externo (la cadena de scope) es el `[[Environment]]` capturado. Esto permite acceder a las variables de la función padre aunque esta ya haya retornado.

```
[Function object]
  ├── [[Call]] → código ejecutable
  ├── [[Environment]] → referencia al entorno léxico donde fue creada
  ├── .name
  ├── .length
  └── .prototype (para uso con new)
```

> [!tip] Buenas prácticas
> - Preferir funciones puras (sin efectos secundarios) como argumentos de HOFs: son más predecibles y testeables.
> - Nombrar los callbacks incluso cuando se pasan inline: `array.filter(esPositivo)` es más legible que `array.filter(n => n > 0)` cuando el predicado tiene nombre semántico.
> - En closures, tener claro qué variables se capturan: una referencia, no una copia. Si la variable cambia después de crear el closure, el closure verá el nuevo valor.

> [!warning] Errores comunes
> **Closure captura referencia, no valor** — el bug clásico con `var` en bucles:
> ```js
> const callbacks = [];
> for (var i = 0; i < 3; i++) {
>   callbacks.push(() => console.log(i)); // captura la variable i, no el valor
> }
> callbacks[0](); // → 3  (no 0)
> callbacks[1](); // → 3  (no 1)
>
> // Solución: let en lugar de var (block scope por iteración)
> for (let i = 0; i < 3; i++) {
>   callbacks.push(() => console.log(i));
> }
> callbacks[0](); // → 0
> ```
> **Perder `this` al pasar un método como callback** — la función pierde su contexto de invocación:
> ```js
> class Timer {
>   constructor() { this.ticks = 0; }
>   tick() { this.ticks++; }
> }
> const t = new Timer();
> setInterval(t.tick, 1000); // → Error: this es undefined (o window)
> setInterval(t.tick.bind(t), 1000); // correcto
> setInterval(() => t.tick(), 1000); // también correcto
> ```

## Notas relacionadas

- [[07 Funciones (básico)/index | Funciones (básico)]]
- [[02 Expresión de Función]]
- [[07 Return]]
- [[JavaScript/03 Programación Funcional/index | Programación Funcional]]
- [[JavaScript/03 Programación Funcional/02 Funciones de Orden Superior/index | Funciones de Orden Superior]]
- [[JavaScript/03 Programación Funcional/06 Currying y Composición/index | Currying y Composición]]
- [[JavaScript/12 Patrones y Buenas Prácticas/04 Memoización | Memoización]]
- [[JavaScript/02 Programación Orientada a Objetos/05 Encapsulación y Abstracción/01 Closures para Datos Privados | Closures para Datos Privados]]
