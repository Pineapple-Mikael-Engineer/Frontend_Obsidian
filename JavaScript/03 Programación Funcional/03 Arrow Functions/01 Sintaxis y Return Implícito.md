---
title: Sintaxis de Arrow Functions y Return Implícito
aliases:
  - sintaxis arrow
  - return implícito
  - implicit return
  - concise body
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
---

# Sintaxis y Return Implícito

> [!definicion]
> Las arrow functions admiten dos formas de cuerpo: **cuerpo de bloque** (`{ ... }`) que requiere `return` explícito, y **cuerpo de expresión** (sin llaves) donde la expresión única se devuelve automáticamente — **return implícito**. La elección de la forma depende de cuántas líneas ocupa la lógica y de si se necesita legibilidad o concisión.

```js
// Cuerpo de bloque — return explícito
const procesar = (x) => {
  const resultado = x * 2 + 1;
  return resultado;
};

// Cuerpo de expresión — return implícito
const procesar = x => x * 2 + 1;
```

## Formas de parámetros

| Nº de parámetros | Paréntesis | Ejemplo |
|---|---|---|
| Cero | Obligatorio | `() => 42` |
| Uno | Opcional | `x => x * 2` o `(x) => x * 2` |
| Dos o más | Obligatorio | `(x, y) => x + y` |
| Con valor por defecto | Obligatorio | `(x, base = 10) => x + base` |
| Con destructuring | Obligatorio | `({ a, b }) => a + b` |
| Rest | Obligatorio | `(...args) => args` |

Con exactamente un parámetro simple, omitir los paréntesis es opcional pero común por convención de brevedad.

## Return implícito — reglas y trampas

El return implícito funciona cuando el cuerpo es **una única expresión** sin llaves. El motor envuelve esa expresión en un `return` al compilar.

```js
// Expresiones válidas para return implícito
const cuadrado  = x => x ** 2;
const saludar   = nombre => `Hola, ${nombre}`;
const esPositivo = n => n > 0;
const primero   = arr => arr[0];
```

**La trampa del objeto literal:** las llaves `{}` se interpretan como cuerpo de bloque, no como literal de objeto. Para devolver un objeto con return implícito, hay que envolverlo en paréntesis:

```js
// ERROR — las llaves se leen como bloque de función, no como objeto
const crear = x => { clave: x };   // devuelve undefined

// CORRECTO — paréntesis fuerzan la interpretación como expresión
const crear = x => ({ clave: x });

// Objeto con varias propiedades
const crearUsuario = (nombre, edad) => ({ nombre, edad, activo: true });
```

## Cuerpo de múltiples líneas

Cuando la lógica requiere más de una expresión, variables intermedias o condicionales, se usa cuerpo de bloque con `return` explícito:

```js
const clasificar = x => {
  if (x < 0) return "negativo";
  if (x === 0) return "cero";
  return "positivo";
};

const normalizarTexto = str => {
  const limpio  = str.trim();
  const lower   = limpio.toLowerCase();
  return lower.replace(/\s+/g, "-");
};
```

## Destructuring de parámetros

El parámetro puede incluir patrones de destructuring directamente en la firma, lo que hace las arrows especialmente limpias con arrays de objetos:

```js
// Destructuring de objeto
const describir = ({ nombre, edad }) => `${nombre} tiene ${edad} años`;

// Con valor por defecto en el patrón
const punto = ({ x = 0, y = 0 }) => `(${x}, ${y})`;

// Destructuring de array
const cabeza = ([primero]) => primero;
const cola   = ([, ...resto]) => resto;

// Uso real: transformar array de objetos
const usuarios = [
  { nombre: "Ana", edad: 30 },
  { nombre: "Luis", edad: 25 },
];
const nombres = usuarios.map(({ nombre }) => nombre);
// → ["Ana", "Luis"]
```

## Rest params

Las arrows admiten rest params con la misma sintaxis que las funciones regulares. Es la alternativa correcta a `arguments` en arrows:

```js
const sumar   = (...nums) => nums.reduce((acc, n) => acc + n, 0);
const unir    = (sep, ...partes) => partes.join(sep);
const primeros = (n, ...items) => items.slice(0, n);

sumar(1, 2, 3, 4);       // → 10
unir("-", "a", "b", "c"); // → "a-b-c"
primeros(2, "x", "y", "z"); // → ["x", "y"]
```

## Recetas comunes

### Transformadores de array inline

```js
const precios    = [10, 25, 8, 42];
const conIVA     = precios.map(p => p * 1.21);
const caros      = precios.filter(p => p > 20);
const total      = precios.reduce((acc, p) => acc + p, 0);
const ordenados  = [...precios].sort((a, b) => a - b);
```

### Callbacks de sort con múltiples criterios

```js
const empleados = [
  { nombre: "Carlos", dpto: "TI",   salario: 50000 },
  { nombre: "Ana",    dpto: "TI",   salario: 60000 },
  { nombre: "Luis",   dpto: "RRHH", salario: 45000 },
];

// Ordenar por departamento, luego por salario descendente
const ordenado = [...empleados].sort((a, b) =>
  a.dpto.localeCompare(b.dpto) || b.salario - a.salario
);
```

### Funciones de mapeado para transformación de datos

```js
// Pipeline de transformación
const pipeline = datos =>
  datos
    .filter(({ activo }) => activo)
    .map(({ id, nombre }) => ({ id, etiqueta: nombre.toUpperCase() }))
    .sort((a, b) => a.etiqueta.localeCompare(b.etiqueta));
```

### Arrow que devuelve arrow (currificación manual)

```js
const multiplicar = factor => numero => numero * factor;
const triple  = multiplicar(3);
const decuple = multiplicar(10);

triple(5);   // → 15
decuple(7);  // → 70

[1, 2, 3].map(multiplicar(2)); // → [2, 4, 6]
```

## Cómo funciona por dentro

El parser de JavaScript reconoce `=>` como marcador de función flecha. Si el cuerpo no está delimitado por llaves, el motor lo trata como **cuerpo conciso** y envuelve la expresión en un `return` implícito durante la compilación a bytecode. La función resultante es un objeto interno de tipo _Arrow Function Exotic Object_, que comparte con `Function` el mecanismo de invocación pero omite la creación de un `[[ThisValue]]` propio, un objeto `arguments` y la propiedad `prototype`. El nombre de la función se infiere del lado izquierdo de la asignación o de la clave del objeto donde aparece (`name: "doble"` en `const doble = x => x * 2`).

> [!tip] Buenas prácticas
> - Usa cuerpo de expresión (return implícito) para callbacks de una línea — la concisión mejora la lectura cuando el nombre del método ya describe la intención (`map`, `filter`).
> - Envuelve siempre los objetos literales en paréntesis para return implícito: `x => ({ clave: x })`.
> - Con un solo parámetro simple, omitir los paréntesis es aceptable; con destructuring o defaults, siempre incluirlos.
> - Si la arrow supera dos o tres líneas de lógica real, plantéate extraerla como función nombrada para mejorar los stack traces.

> [!warning] Errores comunes
> - `x => { clave: x }` devuelve `undefined`, no el objeto — las llaves sin paréntesis son un bloque de código.
> - `() => { return }` devuelve `undefined` explícitamente, pero `() => {}` también — ambas formas son equivalentes cuando el cuerpo de bloque no tiene `return` con valor.
> - Mezclar rest params con parámetros después: `(...args, extra)` es un error de sintaxis — el rest siempre va al final.
> - Olvidar que el return implícito no funciona con `void` — si se quiere una arrow que ejecute un efecto sin devolver nada, usar cuerpo de bloque o ignorar el valor devuelto.

## Notas relacionadas

- [[02 Sin this Propio]] — la otra diferencia semántica clave de las arrows respecto a funciones regulares
- [[03 Sin arguments ni new]] — por qué `arguments` no existe en arrows y qué usar en su lugar
- [[04 Cuándo Usarlas]] — guía de decisión entre arrow y función regular
- [[02 Funciones de Orden Superior/index|Funciones de Orden Superior]] — contexto de uso principal de las arrows como callbacks
