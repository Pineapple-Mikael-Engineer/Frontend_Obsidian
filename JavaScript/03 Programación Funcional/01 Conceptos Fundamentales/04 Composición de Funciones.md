---
title: Composición de Funciones — Construir pipelines de transformación
aliases:
  - composición de funciones
  - function composition
  - pipe
  - compose
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
---

# Composición de Funciones

> [!definicion]
> La **composición de funciones** es la técnica de combinar dos o más funciones donde la salida de una es la entrada de la siguiente, produciendo una función nueva que ejecuta la cadena completa. En notación matemática: `(f ∘ g)(x) = f(g(x))` — `g` se aplica primero, luego `f`. En código, esto se implementa con `compose` (derecha a izquierda) o `pipe` (izquierda a derecha).

```js
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);

const limpiar   = s => s.trim();
const minusculas = s => s.toLowerCase();
const sinEspacios = s => s.replace(/\s+/g, "-");

const slugificar = pipe(limpiar, minusculas, sinEspacios);

slugificar("  Hola Mundo  "); // → "hola-mundo"
slugificar("  FooBar  ");     // → "foobar"
```

## `compose` vs `pipe`

| Función | Orden de aplicación | Lectura | Equivale a |
|---|---|---|---|
| `compose(f, g, h)` | h → g → f | derecha a izquierda (matemático) | `x => f(g(h(x)))` |
| `pipe(f, g, h)` | f → g → h | izquierda a derecha (más natural) | `x => h(g(f(x)))` |

```js
// compose: orden matemático (derecha a izquierda)
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);

const doble   = x => x * 2;
const sumarUno = x => x + 1;

const compuesta = compose(doble, sumarUno); // primero sumarUno, luego doble
compuesta(5); // → (5+1)*2 = 12

// pipe: orden de lectura (izquierda a derecha)
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);

const pipeline = pipe(sumarUno, doble); // primero sumarUno, luego doble
pipeline(5); // → (5+1)*2 = 12  — mismo resultado, más legible
```

## Requisito: funciones unarias

Las funciones de `pipe`/`compose` esperan funciones de **aridad 1** (un argumento). Funciones con aridad mayor se adaptan con currificación previa:

```js
// Función de aridad 2 — no componible directamente
const multiplicar = (factor, n) => n * factor;

// Currificada — aridad 1, componible
const multiplicarPor = factor => n => n * factor;

const pipeline = pipe(
  multiplicarPor(3),  // x => x * 3
  multiplicarPor(2),  // x => x * 2
  sumarUno,
);

pipeline(5); // → 5*3*2+1 = 31
```

## Composición con method chaining — el estilo más frecuente en JS

El encadenamiento de métodos de array es la forma de composición más usada en JavaScript. Cada método retorna un nuevo array, que sirve como entrada al siguiente:

```js
const pedidos = [
  { id: 1, importe: 120, pagado: true },
  { id: 2, importe: 45,  pagado: false },
  { id: 3, importe: 200, pagado: true },
];

const totalPagado = pedidos
  .filter(p => p.pagado)           // [pedido1, pedido3]
  .map(p => p.importe)             // [120, 200]
  .reduce((acc, n) => acc + n, 0); // 320

console.log(totalPagado); // → 320
```

La diferencia con `pipe` es que el dato está a la izquierda (receptor del método) y las funciones son métodos del prototipo, no funciones libres. Para reutilizar el pipeline como unidad, `pipe` con funciones libres es más flexible.

## Composición de transformaciones de objetos

```js
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);

const normalizar  = u => ({ ...u, nombre: u.nombre.trim() });
const mayusculas  = u => ({ ...u, nombre: u.nombre.toUpperCase() });
const marcarActivo = u => ({ ...u, activo: true });
const agregarFecha = u => ({ ...u, creadoEn: "2026-06-07" });

const prepararUsuario = pipe(normalizar, mayusculas, marcarActivo, agregarFecha);

prepararUsuario({ nombre: "  ana  ", email: "ana@x.com" });
// → { nombre: "ANA", email: "ana@x.com", activo: true, creadoEn: "2026-06-07" }
```

## Recetas comunes

### Pipe con valor asíncrono — `pipeAsync`

```js
const pipeAsync = (...fns) => x => fns.reduce(async (promesa, f) => f(await promesa), x);

const obtenerUsuario = id => fetch(`/api/usuarios/${id}`).then(r => r.json());
const enriquecerConRol = u => ({ ...u, rol: "admin" });
const formatear = u => `${u.nombre} (${u.rol})`;

const procesarUsuario = pipeAsync(obtenerUsuario, enriquecerConRol, formatear);
const resultado = await procesarUsuario(42); // → "Ana (admin)"
```

### Reutilizar sub-pipelines

```js
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);

const normalizarTexto = pipe(s => s.trim(), s => s.toLowerCase());
const normalizarEmail = pipe(normalizarTexto, s => s.replace(/\s/g, ""));
const normalizarNombre = pipe(normalizarTexto, s => s.replace(/\b\w/g, c => c.toUpperCase()));

// normalizarTexto se reutiliza como bloque en pipelines más específicos
normalizarEmail("  Usuario@X.COM  ");  // → "usuario@x.com"
normalizarNombre("  juan pablo  ");    // → "Juan Pablo"
```

### Point-free style — funciones sin mencionar sus argumentos

```js
// Con argumento explícito
const dobleDelCuadrado = x => doble(cuadrado(x));

// Point-free: la función no menciona su argumento
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);
const dobleDelCuadrado = compose(doble, cuadrado);

dobleDelCuadrado(3); // → doble(cuadrado(3)) = doble(9) = 18
```

El estilo point-free es más declarativo — describe la transformación, no la mecánica — pero puede ser menos legible cuando la composición es muy larga o los nombres de función no son autoexplicativos.

## Cómo funciona por dentro

`pipe` y `compose` son **funciones de orden superior**: reciben funciones como argumentos y retornan una nueva función. No hay magia en el runtime — `pipe(...fns)` retorna `x => fns.reduce((v, f) => f(v), x)`, que al llamarse aplica cada función al resultado de la anterior usando `Array.prototype.reduce`. El valor se transforma iterativamente, sin crear closures adicionales por cada paso. El coste es una llamada a `reduce` más `n` llamadas a función — mínimo.

```js
// Lo que pipe(f, g, h)(x) hace exactamente:
// fns = [f, g, h]
// reduce empieza con v = x
// iteración 1: v = f(x)
// iteración 2: v = g(f(x))
// iteración 3: v = h(g(f(x)))
// retorna h(g(f(x)))
```

> [!tip] Buenas prácticas
> - Nombrar el pipeline resultante con el verbo que describe la transformación completa: `slugificar`, `prepararUsuario`, `calcularTotal` — el pipeline es una función de primera clase con nombre propio.
> - Mantener cada función del pipeline con una responsabilidad: una función que hace dos cosas distintas suele indicar que hay un paso intermedio implícito que conviene explicitar.
> - Preferir `pipe` a `compose` en código JS moderno — el orden de lectura de izquierda a derecha coincide con el orden de ejecución.
> - Cuando el method chaining es suficiente (array transformations), usarlo — es idiomático y no requiere librerías.

> [!warning] Errores comunes
> - **Funciones de aridad mayor que 1 sin currificar**: `pipe(Math.pow)` no funciona como se espera porque `Math.pow(base, exponente)` necesita dos argumentos — currificar primero.
> - **Efectos secundarios en medio del pipeline**: si un paso tiene un efecto (log, mutación), el pipeline deja de ser una cadena de transformaciones puras y se vuelve difícil de razonar. Los efectos van al final o se inyectan.
> - **Pipelines demasiado largos**: si un pipeline tiene más de 6–8 pasos, probablemente conviene dividirlo en sub-pipelines nombrados para mantener la legibilidad.
> - **Confundir `compose(f, g)` y `pipe(f, g)`**: en `compose` se aplica `g` primero; en `pipe` se aplica `f` primero. Intercambiarlos produce resultados incorrectos silenciosamente si los tipos coinciden.

## Notas relacionadas

- [[01 Funciones Puras]] — solo las funciones puras se componen de forma predecible.
- [[05 Declarativo vs Imperativo]] — la composición es el mecanismo que hace posible el estilo declarativo.
- [[02 Funciones de Orden Superior/index|Funciones de Orden Superior]] — `pipe` y `compose` son funciones de orden superior; `map`, `filter` y `reduce` son los bloques de composición más usados en JS.
