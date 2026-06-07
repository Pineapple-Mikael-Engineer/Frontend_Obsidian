---
title: Declarativo vs Imperativo — Qué se quiere vs cómo hacerlo
aliases:
  - declarativo vs imperativo
  - estilo declarativo
  - estilo imperativo
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
---

# Declarativo vs Imperativo

> [!definicion]
> El **estilo imperativo** describe **cómo** se resuelve un problema: pasos explícitos, variables de estado intermedias, bucles con índices, asignaciones sucesivas. El **estilo declarativo** describe **qué** se quiere obtener y delega el cómo a una función, método o motor. La programación funcional favorece el estilo declarativo: se especifica la transformación, no la mecánica de ejecución.

```js
const numeros = [1, 2, 3, 4, 5, 6];

// Imperativo: describe cómo
const pares = [];
for (let i = 0; i < numeros.length; i++) {
  if (numeros[i] % 2 === 0) pares.push(numeros[i]);
}

// Declarativo: describe qué
const pares = numeros.filter(n => n % 2 === 0);

// Ambos producen → [2, 4, 6]
```

## Comparaciones directas

### Transformar — `map`

```js
const precios = [10, 25, 80, 5];

// Imperativo
const conIVA = [];
for (let i = 0; i < precios.length; i++) {
  conIVA.push(precios[i] * 1.21);
}

// Declarativo
const conIVA = precios.map(p => p * 1.21);
// → [12.1, 30.25, 96.8, 6.05]
```

### Acumular — `reduce`

```js
const importes = [120, 45, 200, 15];

// Imperativo
let total = 0;
for (let i = 0; i < importes.length; i++) {
  total += importes[i];
}

// Declarativo
const total = importes.reduce((acc, n) => acc + n, 0);
// → 380
```

### Buscar — `find` / `findIndex`

```js
const usuarios = [
  { id: 1, nombre: "Ana" },
  { id: 2, nombre: "Luis" },
  { id: 3, nombre: "Eva" },
];

// Imperativo
let encontrado = null;
for (let i = 0; i < usuarios.length; i++) {
  if (usuarios[i].id === 2) { encontrado = usuarios[i]; break; }
}

// Declarativo
const encontrado = usuarios.find(u => u.id === 2);
// → { id: 2, nombre: "Luis" }
```

### Comprobar condición — `some` / `every`

```js
const edades = [22, 17, 30, 15];

// Imperativo
let hayMenor = false;
for (let i = 0; i < edades.length; i++) {
  if (edades[i] < 18) { hayMenor = true; break; }
}

// Declarativo
const hayMenor = edades.some(e => e < 18); // → true
const todosMayores = edades.every(e => e >= 18); // → false
```

### Aplanar y transformar — `flatMap`

```js
const pedidos = [
  { cliente: "Ana", items: ["libro", "lapiz"] },
  { cliente: "Luis", items: ["cuaderno"] },
];

// Imperativo
const todosItems = [];
for (const pedido of pedidos) {
  for (const item of pedido.items) {
    todosItems.push(item);
  }
}

// Declarativo
const todosItems = pedidos.flatMap(p => p.items);
// → ["libro", "lapiz", "cuaderno"]
```

## Tabla comparativa de estilos

| Aspecto | Imperativo | Declarativo |
|---|---|---|
| Enfoque | Cómo (pasos) | Qué (resultado) |
| Estado intermediario | Variables locales explícitas | Encapsulado en el método |
| Legibilidad | Depende de la complejidad | Expresa la intención directamente |
| Composición | Anidamiento o secuencia de pasos | Method chaining, pipe |
| Control de flujo | `break`, `continue`, `return` tempranos | Predicados en `filter`, `find`, `some` |
| Rendimiento | Más control (evitar arrays intermedios) | Abstracción con coste mínimo |
| Depuración | Variables intermedias visibles | Difícil inspeccionar pasos intermedios |

## Cuándo elegir imperativo

El estilo imperativo es preferible cuando la lógica de ruptura es compleja (`break`/`continue` con múltiples condiciones), cuando el rendimiento es crítico y los arrays intermedios de `filter`+`map` deben evitarse, o cuando el algoritmo modifica el índice en marcha:

```js
// Imperativo — lógica de ruptura compleja difícil de expresar de forma declarativa
function encontrarPrimeroValido(items) {
  for (let i = 0; i < items.length; i++) {
    if (!items[i].activo) continue;
    if (items[i].prioridad > 5) break; // salir del grupo
    if (validar(items[i])) return items[i]; // retorno temprano
  }
  return null;
}

// Imperativo — performance: un único recorrido sin arrays intermedios
function sumarParesMayores(nums, umbral) {
  let suma = 0;
  for (const n of nums) {
    if (n % 2 === 0 && n > umbral) suma += n;
  }
  return suma;
}

// En declarativo se crearían dos arrays intermedios: filter + map
// Para millones de elementos, el imperativo es O(n) sin allocations
```

## Cuándo elegir declarativo

El estilo declarativo es preferible en transformaciones de datos, pipelines de negocio, código que debe leerse como especificación, y cualquier caso donde la intención sea más importante que los detalles de ejecución:

```js
// Pipeline declarativo — legible como especificación de negocio
const resumen = pedidos
  .filter(p => p.estado === "completado")
  .map(p => ({ ...p, total: p.items.reduce((s, i) => s + i.precio, 0) }))
  .sort((a, b) => b.total - a.total)
  .slice(0, 10);
// "Los 10 pedidos completados con mayor importe"
```

## Niveles de declaratividad en JS

El lenguaje tiene múltiples capas de declaratividad. De más imperativo a más declarativo:

```js
// Nivel 1: bucle clásico — completamente imperativo
for (let i = 0; i < arr.length; i++) { ... }

// Nivel 2: for…of — elimina el índice, más declarativo
for (const item of arr) { ... }

// Nivel 3: métodos de array — abstrae el bucle
arr.forEach(item => { ... });

// Nivel 4: transformaciones encadenadas — expresa la intención
arr.filter(...).map(...).reduce(...);

// Nivel 5: composición nombrada — el nombre es la especificación
const calcularTotal = pipe(filtrarActivos, mapearImportes, sumar);
```

## Cómo funciona por dentro

Los métodos declarativos de array como `map`, `filter` y `reduce` son implementados internamente como bucles. `Array.prototype.map` itera el array de `0` a `length - 1`, llama al callback por cada índice que existe (saluda posiciones huecas) y construye un nuevo array con los resultados. El coste de la abstracción es una llamada de función extra por elemento — mínimo en la mayoría de casos, relevante solo en hot paths de millones de iteraciones con V8 en modo de máxima optimización.

> [!tip] Buenas prácticas
> - Preferir el estilo declarativo por defecto — expresa la intención y es más fácil de componer.
> - Nombrar los predicados cuando son complejos: `const esMayor = n => n >= 18` antes de pasar a `filter`, en lugar de una función anónima larga.
> - Evitar `forEach` cuando el propósito es transformar — `forEach` es para efectos secundarios; `map` es para transformaciones.
> - Cuando la legibilidad del estilo declarativo se pierde por excesivo anidamiento de callbacks, refactorizar a variables con nombre o cambiar a imperativo.

> [!warning] Errores comunes
> - **Usar `map` para efectos secundarios**: `arr.map(x => { console.log(x); })` retorna un array de `undefined` — usar `forEach` cuando el propósito es el efecto, no el valor de retorno.
> - **`reduce` como herramienta universal**: es tentador resolver todo con `reduce`, pero `filter` + `map` es más legible cuando la transformación lo permite — reservar `reduce` para acumulaciones complejas.
> - **Cadenas que crean múltiples arrays intermedios innecesariamente**: `.filter().map().filter().map()` puede reemplazarse por una sola pasada con `reduce` si el rendimiento es crítico.
> - **Point-free forzado**: no toda función necesita ser point-free — si el argumento explícito hace la intención más clara, mantenerlo.

## Notas relacionadas

- [[04 Composición de Funciones]] — la composición es el mecanismo que hace posible el estilo declarativo con funciones libres.
- [[01 Funciones Puras]] — el estilo declarativo asume que las funciones en el pipeline no tienen efectos secundarios.
- [[02 Funciones de Orden Superior/index|Funciones de Orden Superior]] — `map`, `filter`, `reduce` son funciones de orden superior que habilitan el estilo declarativo en arrays.
