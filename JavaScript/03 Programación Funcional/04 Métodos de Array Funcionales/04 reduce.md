---
title: "reduce"
aliases:
  - Array reduce
  - array.reduce
tags:
  - javascript
  - api/metodo
  - arrays
draft: false
---

# reduce

> [!definicion]
> `Array.prototype.reduce` recorre el array de izquierda a derecha aplicando un callback que combina cada elemento con un **acumulador**, produciendo un único valor final de cualquier tipo. Es el método funcional más genérico: con `reduce` se pueden implementar `map`, `filter`, `find`, `some` y prácticamente cualquier transformación de array.

```js
const numeros = [1, 2, 3, 4, 5];
const suma = numeros.reduce((acum, n) => acum + n, 0);
// 15

// Sin valor inicial → usa el primer elemento como acumulador
const producto = [2, 3, 4].reduce((acum, n) => acum * n);
// 24  (empieza con acum=2, luego 2*3=6, luego 6*4=24)
```

## Firma del método

| Parámetro del callback | Tipo | Descripción |
|---|---|---|
| `acumulador` | any | Valor acumulado hasta la iteración anterior (o `valorInicial` en la primera) |
| `elemento` | any | Valor del elemento actual |
| `índice` | number | Posición del elemento actual (0 o 1 según haya `valorInicial`) |
| `array` | Array | Referencia al array original |

| Aspecto | Detalle |
|---|---|
| Valor de retorno del método | El valor final del acumulador (cualquier tipo) |
| `valorInicial` omitido | Usa el elemento en índice 0 como acumulador; itera desde índice 1 |
| Array vacío sin `valorInicial` | Lanza `TypeError` |
| Array de un elemento sin `valorInicial` | Devuelve ese elemento sin invocar el callback |
| Muta el array | No |

## Por qué siempre proporcionar `valorInicial`

```js
// Array vacío sin valorInicial → TypeError en tiempo de ejecución
[].reduce((a, b) => a + b);
// TypeError: Reduce of empty array with no initial value

// Con valorInicial → devuelve el valor inicial
[].reduce((a, b) => a + b, 0);
// 0

// Sin valorInicial, el tipo del acumulador depende del primer elemento
// → difícil de razonar sobre el tipo en TypeScript y en código genérico
```

Proporcionar siempre `valorInicial` hace que el tipo del acumulador sea predecible desde el inicio, evita errores con arrays vacíos y hace el código más fácil de leer.

## Ejemplos

### Suma y promedio

```js
const notas = [7, 8, 9, 6, 10];
const total = notas.reduce((sum, n) => sum + n, 0);
// 40
const promedio = total / notas.length;
// 8
```

### Contar ocurrencias

```js
const palabras = ["manzana", "banana", "manzana", "cereza", "banana", "manzana"];

const conteo = palabras.reduce((acc, palabra) => {
  acc[palabra] = (acc[palabra] ?? 0) + 1;
  return acc;
}, {});
// {manzana: 3, banana: 2, cereza: 1}
```

### Agrupar objetos por propiedad (groupBy)

```js
const personas = [
  { nombre: "Ana", ciudad: "Madrid" },
  { nombre: "Luis", ciudad: "Barcelona" },
  { nombre: "Eva", ciudad: "Madrid" },
  { nombre: "Tomás", ciudad: "Sevilla" },
];

const porCiudad = personas.reduce((acc, persona) => {
  const key = persona.ciudad;
  if (!acc[key]) acc[key] = [];
  acc[key].push(persona);
  return acc;
}, {});
// {
//   Madrid: [{nombre: "Ana",...}, {nombre: "Eva",...}],
//   Barcelona: [{nombre: "Luis",...}],
//   Sevilla: [{nombre: "Tomás",...}]
// }
```

### Construir objeto desde array de pares (alternativa a `Object.fromEntries`)

```js
const pares = [["a", 1], ["b", 2], ["c", 3]];

const obj = pares.reduce((acc, [clave, valor]) => {
  acc[clave] = valor;
  return acc;
}, {});
// {a: 1, b: 2, c: 3}
```

### Aplanar un nivel (implementar `flat(1)`)

```js
const anidado = [[1, 2], [3, 4], [5, 6]];
const plano = anidado.reduce((acc, arr) => acc.concat(arr), []);
// [1, 2, 3, 4, 5, 6]
```

### Pipeline de funciones (implementar `pipe`)

```js
const pipe = (...fns) => (x) => fns.reduce((v, f) => f(v), x);

const doble = (n) => n * 2;
const sumarUno = (n) => n + 1;
const alCuadrado = (n) => n ** 2;

const transformar = pipe(doble, sumarUno, alCuadrado);
transformar(3);
// doble(3)=6 → sumarUno(6)=7 → alCuadrado(7)=49
```

## Recetas comunes

### Implementar `map` con `reduce`

```js
const mapConReduce = (arr, fn) =>
  arr.reduce((acc, elem, i) => {
    acc.push(fn(elem, i, arr));
    return acc;
  }, []);
```

### Implementar `filter` con `reduce`

```js
const filterConReduce = (arr, predicado) =>
  arr.reduce((acc, elem, i) => {
    if (predicado(elem, i, arr)) acc.push(elem);
    return acc;
  }, []);
```

### Calcular estadísticas en una pasada

```js
const stats = [4, 8, 2, 10, 6].reduce(
  (acc, n) => ({
    min: Math.min(acc.min, n),
    max: Math.max(acc.max, n),
    suma: acc.suma + n,
    count: acc.count + 1,
  }),
  { min: Infinity, max: -Infinity, suma: 0, count: 0 }
);

const resultado = { ...stats, promedio: stats.suma / stats.count };
// {min: 2, max: 10, suma: 30, count: 5, promedio: 6}
```

### Reducir un array de promesas en secuencia (ejecución serial)

```js
const ejecutarEnSerie = (fns) =>
  fns.reduce(
    (promesaAnterior, fn) => promesaAnterior.then(fn),
    Promise.resolve()
  );

ejecutarEnSerie([paso1, paso2, paso3]);
// paso1 → (espera) → paso2 → (espera) → paso3
```

## Cómo funciona por dentro

`reduce` itera desde el índice `0` (o `1` si no hay `valorInicial`) hasta `length - 1`. En cada iteración invoca `callback(acumulador, elemento, índice, array)` y asigna el valor devuelto como nuevo `acumulador`. Al terminar, devuelve el acumulador final.

```
[1, 2, 3, 4].reduce((acc, n) => acc + n, 0)

Iteración 1: callback(0, 1, 0, arr) → devuelve 1  → acum = 1
Iteración 2: callback(1, 2, 1, arr) → devuelve 3  → acum = 3
Iteración 3: callback(3, 3, 2, arr) → devuelve 6  → acum = 6
Iteración 4: callback(6, 4, 3, arr) → devuelve 10 → acum = 10
Resultado: 10
```

Sin `valorInicial`:

```
[1, 2, 3, 4].reduce((acc, n) => acc + n)

acum inicial = arr[0] = 1, se empieza en índice 1
Iteración 1: callback(1, 2, 1, arr) → 3
Iteración 2: callback(3, 3, 2, arr) → 6
Iteración 3: callback(6, 4, 3, arr) → 10
Resultado: 10
```

> [!tip] Buenas prácticas
> - Proporcionar siempre `valorInicial` — hace el comportamiento predecible con arrays vacíos y el tipo del acumulador claro desde el inicio.
> - Devolver siempre el acumulador al final del callback — olvidarlo hace que el acumulador sea `undefined` en la siguiente iteración (bug silencioso).
> - Usar `reduce` para transformaciones que necesitan acumular estado complejo. Para transformaciones simples elemento-a-elemento, `map` o `filter` son más legibles.
> - Nombrar el acumulador según lo que representa: `suma`, `grupos`, `porId`, `resultado` — no simplemente `acc` en código de producción.
> - Para pipelines de izquierda a derecha, usar `reduce`; para de derecha a izquierda (composición), usar `reduceRight`.

> [!warning] Errores comunes
> - **Olvidar `return` en el callback:** el acumulador se convierte en `undefined` en la siguiente iteración. Es el error más frecuente con `reduce`.
> - **Array vacío sin `valorInicial`:** lanza `TypeError`. Proporcionar siempre `valorInicial`.
> - **Mutar el acumulador directamente sin retornarlo:** en `(acc, x) => { acc.push(x); }` el `push` muta pero no hay `return`, así que el acumulador pasa a ser `undefined`.
> - **Usar `reduce` cuando `map`/`filter` son más claros:** `reduce` puede hacer todo, pero no siempre es el más legible. Usar el método más específico para la tarea.
> - **No inicializar el acumulador con el tipo correcto:** si el resultado es un objeto, inicializar con `{}`; si es un array, con `[]`; si es un número, con `0`.

## Notas relacionadas

- [[index|Métodos de Array Funcionales — Índice]]
- [[02 map]]
- [[03 filter]]
- [[05 reduceRight]]
- [[12 Encadenamiento de Métodos]]
