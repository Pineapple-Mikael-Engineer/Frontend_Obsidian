---
title: compose — Composición de funciones (derecha a izquierda)
aliases:
  - compose
  - composición de funciones
  - function composition
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
order: 3
---

# compose

> [!definicion]
> `compose(f, g, h)(x)` aplica las funciones de **derecha a izquierda**: primero `h(x)`, luego `g` sobre ese resultado, y finalmente `f`. El resultado es `f(g(h(x)))`. Refleja la notación matemática de composición `(f ∘ g ∘ h)(x) = f(g(h(x)))`. Las funciones deben ser **unarias** — la salida de cada una es la entrada de la siguiente.

```js
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);

const doble  = x => x * 2;
const sumar1 = x => x + 1;
const cuadrado = x => x ** 2;

const transformar = compose(doble, sumar1, cuadrado);
// equivale a: x => doble(sumar1(cuadrado(x)))

transformar(3);
// cuadrado(3) = 9 → sumar1(9) = 10 → doble(10) = 20
```

## Implementación

```js
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);
```

`reduceRight` itera el array de funciones de derecha a izquierda, acumulando el resultado de aplicar cada función al valor anterior. El valor inicial del acumulador es `x`.

## Orden de ejecución: derecha a izquierda

El orden en que se escriben las funciones en `compose` es el **inverso** al orden de ejecución. La primera en ejecutarse es la que está más a la derecha.

```js
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);

const trim     = s => s.trim();
const minusc   = s => s.toLowerCase();
const normalizar = s => s.replace(/\s+/g, "-");

// Lee de derecha a izquierda: primero trim, luego minúsculas, luego normalizar
const slug = compose(normalizar, minusc, trim);

slug("  Hola Mundo  ");  // "hola-mundo"
```

Para muchos programadores, el orden de lectura es contraintuitivo. La alternativa de lectura natural es [[04 pipe]].

## Composición con funciones currificadas

El poder de `compose` se multiplica cuando las funciones de la cadena son funciones currificadas o con aplicación parcial — cada función puede recibir configuración antes de entrar al pipeline.

```js
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);

const map    = fn => arr => arr.map(fn);
const filter = fn => arr => arr.filter(fn);

const procesarProductos = compose(
  map(p => ({ ...p, total: p.precio * p.cantidad })),
  filter(p => p.activo),
  filter(p => p.stock > 0)
);

const productos = [
  { nombre: "A", precio: 10, cantidad: 3, stock: 5, activo: true },
  { nombre: "B", precio: 20, cantidad: 1, stock: 0, activo: true },
  { nombre: "C", precio: 15, cantidad: 2, stock: 3, activo: false },
  { nombre: "D", precio: 8,  cantidad: 4, stock: 2, activo: true },
];

procesarProductos(productos);
// [
//   { nombre: "A", precio: 10, cantidad: 3, stock: 5, activo: true, total: 30 },
//   { nombre: "D", precio: 8,  cantidad: 4, stock: 2, activo: true, total: 32 },
// ]
```

## Recetas

### Pipeline de transformación de texto

```js
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);

const trim      = s => s.trim();
const split     = sep => s => s.split(sep);
const join      = sep => arr => arr.join(sep);
const capitalize = s => s.charAt(0).toUpperCase() + s.slice(1);
const mapArr    = fn => arr => arr.map(fn);

const titleCase = compose(
  join(" "),
  mapArr(capitalize),
  split(" "),
  trim
);

titleCase("  hola mundo cruel  ");  // "Hola Mundo Cruel"
```

### Validación en capas

```js
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);

const noVacio    = s => s.length > 0 ? { ok: true, valor: s } : { ok: false, error: "vacío" };
const minLen     = n => result => result.ok && result.valor.length < n
  ? { ok: false, error: `mínimo ${n} caracteres` }
  : result;
const sinEspacios = result => result.ok && /\s/.test(result.valor)
  ? { ok: false, error: "no puede tener espacios" }
  : result;

const validarUser = compose(sinEspacios, minLen(4), noVacio);

validarUser("Ana");         // { ok: false, error: "mínimo 4 caracteres" }
validarUser("Ana García");  // { ok: false, error: "no puede tener espacios" }
validarUser("anamaria");    // { ok: true, valor: "anamaria" }
```

## Composición de dos funciones: verificación matemática

```js
// (f ∘ g)(x) = f(g(x))
const f = x => x + 1;
const g = x => x * 2;

const fg = compose(f, g);  // f(g(x))
fg(3);  // g(3)=6, f(6)=7

// Composición no es conmutativa en general
const gf = compose(g, f);  // g(f(x))
gf(3);  // f(3)=4, g(4)=8
```

## Cómo funciona por dentro

```js
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);
```

`fns.reduceRight` toma el array de funciones y lo recorre de derecha a izquierda. El valor inicial del acumulador `v` es el argumento de entrada `x`. En cada paso, aplica la función `f` al valor acumulado hasta ese momento. Al terminar, `v` contiene el resultado de haber aplicado todas las funciones en orden de derecha a izquierda.

Es equivalente a la versión recursiva:

```js
const compose2 = (f, g) => x => f(g(x));
const compose3 = (f, g, h) => x => f(g(h(x)));
// compose generaliza este patrón para N funciones
```

> [!tip] Buenas prácticas
> - Usar `compose` cuando el orden matemático (`f ∘ g`) es la forma natural de describir la transformación en el dominio del problema — frecuente en matemáticas y procesamiento de señales.
> - Preferir [[04 pipe]] cuando el equipo describe los pasos en orden cronológico natural (primero A, luego B, luego C).
> - Nombrar la función compuesta con el dominio del resultado: `const slug = compose(normalizar, minusc, trim)` — el nombre describe qué produce, no los pasos.
> - Las funciones en la cadena deben ser unarias y puras; mezclar funciones con efectos secundarios en un `compose` dificulta el razonamiento.

> [!warning] Errores comunes
> - **Orden contra-intuitivo:** la primera función en ejecutarse es la última en el código. Es la causa más frecuente de bugs al usar `compose`. Si el orden natural importa para el equipo, usar `pipe`.
> - **Funciones no unarias en la cadena:** si una función en el medio espera dos argumentos, `compose` no puede encadenarla directamente — currificarla primero.
> - **Propagación de `undefined`:** si una función en la cadena no retorna explícitamente, devuelve `undefined`, que se convierte en la entrada de la siguiente. Verificar que todas las funciones retornan un valor.
> - **Composición de funciones asíncronas:** `compose` estándar no gestiona Promesas. Para pipelines async, usar una versión con `await` o usar [[04 pipe]] con su variante asíncrona.

## Notas relacionadas

- [[index|Currying y Composición — Índice]]
- [[04 pipe]]
- [[01 Currying]]
- [[02 Partial Application]]
