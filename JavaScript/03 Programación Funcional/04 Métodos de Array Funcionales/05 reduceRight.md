---
title: "reduceRight"
aliases:
  - Array reduceRight
  - array.reduceRight
tags:
  - javascript
  - api/metodo
  - arrays
draft: false
---

# reduceRight

> [!definicion]
> `Array.prototype.reduceRight` funciona exactamente igual que `reduce`, pero itera el array **de derecha a izquierda** — desde el último índice hacia el primero. Es esencial cuando el orden de aplicación de las operaciones importa, como en la composición de funciones matemáticas o en la inversión de pipelines.

```js
const numeros = [1, 2, 3, 4, 5];

// reduce: izquierda → derecha
const reduceIzq = numeros.reduce((acc, n) => `(${acc}+${n})`, "0");
// "(((((0+1)+2)+3)+4)+5)"

// reduceRight: derecha → izquierda
const reduceDer = numeros.reduceRight((acc, n) => `(${acc}+${n})`, "0");
// "(((((0+5)+4)+3)+2)+1)"
```

## Firma del método

| Parámetro del callback | Tipo | Descripción |
|---|---|---|
| `acumulador` | any | Valor acumulado de la iteración anterior |
| `elemento` | any | Valor del elemento actual (se visita de derecha a izquierda) |
| `índice` | number | Posición del elemento actual |
| `array` | Array | Referencia al array original |

| Aspecto | Detalle |
|---|---|
| Valor de retorno del método | El valor final del acumulador |
| Orden de iteración | De `length - 1` hacia `0` |
| `valorInicial` omitido | Usa el último elemento como acumulador, empieza en `length - 2` |
| Array vacío sin `valorInicial` | Lanza `TypeError` |
| Muta el array | No |

## Cuándo importa el orden: operaciones no conmutativas

Para operaciones conmutativas (suma, multiplicación de números), `reduce` y `reduceRight` dan el mismo resultado. La diferencia solo importa con operaciones no conmutativas:

```js
// Concatenación de strings (no conmutativa)
const palabras = ["hola", " ", "mundo"];

palabras.reduce((acc, s) => acc + s, "");
// "hola mundo"

palabras.reduceRight((acc, s) => acc + s, "");
// "mundo hola"

// División (no conmutativa)
const nums = [64, 4, 2];

nums.reduce((acc, n) => acc / n, 256);
// 256/64=4 → 4/4=1 → 1/2=0.5

nums.reduceRight((acc, n) => acc / n, 256);
// 256/2=128 → 128/4=32 → 32/64=0.5  (distinto proceso, mismo resultado en este caso)
```

## Caso de uso principal: `compose`

La composición matemática de funciones se lee de derecha a izquierda: `(f ∘ g)(x) = f(g(x))`. `reduceRight` implementa esto de forma natural:

```js
const compose = (...fns) => (x) => fns.reduceRight((v, f) => f(v), x);

const doblar = (n) => n * 2;
const sumarDiez = (n) => n + 10;
const alCuadrado = (n) => n ** 2;

// compose(f, g, h)(x) = f(g(h(x)))
const transformar = compose(doblar, sumarDiez, alCuadrado);
transformar(3);
// alCuadrado(3)=9 → sumarDiez(9)=19 → doblar(19)=38
```

Comparado con `pipe` (que usa `reduce` de izquierda a derecha):

```js
const pipe = (...fns) => (x) => fns.reduce((v, f) => f(v), x);

const transformarPipe = pipe(alCuadrado, sumarDiez, doblar);
transformarPipe(3);
// alCuadrado(3)=9 → sumarDiez(9)=19 → doblar(19)=38
// Mismo resultado, funciones en orden inverso
```

## Recetas comunes

### Invertir el orden de procesamiento de un pipeline

```js
const pasosDeProcesamiento = [normalizarTexto, eliminarStopwords, tokenizar];

// Procesamiento normal (izquierda a derecha)
const procesarNormal = pipe(...pasosDeProcesamiento);

// Procesamiento inverso (deshacer pasos en orden contrario)
const deshacerPasos = [detokenizar, añadirStopwords, desnormalizar];
const deshacerProcesamiento = compose(...deshacerPasos);
// compose usa reduceRight internamente
```

### Aplicar middleware de derecha a izquierda (patrón Redux-like)

```js
const applyMiddleware = (...middlewares) => (store) =>
  middlewares.reduceRight(
    (next, middleware) => middleware(store)(next),
    store.dispatch
  );
```

### Aplanar arrays anidados de derecha a izquierda

```js
const anidado = [[1, 2], [3, 4], [5, 6]];

const planoIzq = anidado.reduce((acc, arr) => acc.concat(arr), []);
// [1, 2, 3, 4, 5, 6]

const planoDer = anidado.reduceRight((acc, arr) => acc.concat(arr), []);
// [5, 6, 3, 4, 1, 2]
```

### Construir un string HTML anidado de interior a exterior

```js
const wrappers = ["section", "article", "p"];
const contenido = "Hola mundo";

const resultado = wrappers.reduceRight(
  (inner, tag) => `<${tag}>${inner}</${tag}>`,
  contenido
);
// "<section><article><p>Hola mundo</p></article></section>"
```

## Cómo funciona por dentro

`reduceRight` itera desde el índice `length - 1` hasta `0`. En cada paso invoca `callback(acumulador, array[i], i, array)` y usa el valor retornado como nuevo acumulador. Si no hay `valorInicial`, el acumulador inicial es `array[length - 1]` y la iteración empieza en `length - 2`.

```
["a", "b", "c"].reduceRight((acc, s) => acc + s, "")

i=2: callback("", "c", 2) → "c"
i=1: callback("c", "b", 1) → "cb"
i=0: callback("cb", "a", 0) → "cba"
Resultado: "cba"
```

El motor salta los huecos en arrays sparse igual que `reduce`.

> [!tip] Buenas prácticas
> - Usar `reduceRight` específicamente cuando el orden de aplicación importa y debe ser de derecha a izquierda. Para transformaciones donde el orden no importa, `reduce` es más familiar y predecible.
> - Documentar explícitamente por qué se usa `reduceRight` en lugar de `reduce` — no es evidente a primera vista.
> - Para implementar `compose`, `reduceRight` es la opción idiomática en JavaScript funcional. Para `pipe`, usar `reduce`.
> - Proporcionar siempre `valorInicial` por las mismas razones que en `reduce`.

> [!warning] Errores comunes
> - **Confundir `reduceRight` con invertir el array:** `reduceRight` invierte el orden de *procesamiento*, no el orden de los elementos en el resultado. Para invertir el array, usar `.reverse()` o `[...arr].reverse()`.
> - **Olvidar el `return` del callback:** mismo bug silencioso que en `reduce` — el acumulador se convierte en `undefined`.
> - **Usar `reduceRight` cuando `reduce` basta:** incrementa la confusión del lector sin beneficio si la operación es conmutativa.
> - **Array vacío sin `valorInicial`:** lanza `TypeError`, igual que `reduce`.

## Notas relacionadas

- [[index|Métodos de Array Funcionales — Índice]]
- [[04 reduce]]
- [[12 Encadenamiento de Métodos]]
