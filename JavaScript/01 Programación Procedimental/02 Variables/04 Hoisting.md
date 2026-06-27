---
title: Hoisting — Elevación de declaraciones en JavaScript
aliases:
  - hoisting
  - elevación de declaraciones
tags:
  - javascript
  - api/concepto
  - variables
tipo: concepto
draft: false
order: 4
---

# Hoisting

> [!definicion]
> El **hoisting** (elevación) es el proceso por el que el motor JavaScript procesa las **declaraciones** antes de ejecutar el código. Durante la fase de creación del contexto de ejecución, el motor recorre el ámbito, registra todos los identificadores declarados y les asigna un estado inicial. La **inicialización** permanece en su lugar original en el código fuente.

```js
console.log(x);  // undefined  ← declaración elevada, no la asignación
var x = 10;
console.log(x);  // 10

saludar();       // "Hola"  ← función completa disponible antes de su línea
function saludar() { console.log("Hola"); }
```

## Comportamiento por tipo de declaración

### `var` — declaración elevada, valor `undefined`

El identificador existe desde el inicio de la función (o del global), pero su valor es `undefined` hasta la línea de asignación.

```js
function demo() {
  console.log(a); // undefined
  var a = 42;
  console.log(a); // 42
}

// Lo que el motor procesa:
function demo() {
  var a;          // elevado al inicio
  console.log(a); // undefined
  a = 42;
  console.log(a); // 42
}
```

### Declaraciones de función — elevación completa

Las *function declarations* se elevan **completas**: el nombre y el cuerpo están disponibles antes de cualquier línea de código.

```js
cuadrado(4); // 16  ← funciona antes de la declaración

function cuadrado(n) {
  return n * n;
}
```

Esto no aplica a *function expressions* ni *arrow functions* asignadas a variables.

### `let` y `const` — elevadas pero en TDZ

El motor conoce los identificadores `let`/`const` desde el inicio del bloque, pero los marca como *uninitialized*. El acceso antes de la declaración lanza `ReferenceError`.

```js
{
  console.log(y); // ReferenceError: Cannot access 'y' before initialization
  let y = 7;
}
```

El período desde el inicio del bloque hasta la línea de declaración es la Temporal Dead Zone. Ver [[05 Temporal Dead Zone]].

### `class` — igual que `let`/`const`

Las declaraciones de clase también están en TDZ hasta su línea.

```js
const obj = new Punto(1, 2); // ReferenceError
class Punto {
  constructor(x, y) { this.x = x; this.y = y; }
}
```

### Function expressions y arrow functions

Solo la variable se eleva (siguiendo las reglas de `var` o `let`/`const`), no la función asignada.

```js
console.log(typeof sumar); // "undefined"  (var elevada sin valor)
var sumar = function(a, b) { return a + b; };

console.log(typeof restar); // ReferenceError  (let en TDZ)
let restar = (a, b) => a - b;
```

## Tabla resumen de comportamientos

| Declaración | ¿Se eleva? | Estado inicial | Acceso antes de la línea |
|---|---|---|---|
| `var` | sí | `undefined` | `undefined` (sin error) |
| `function` declaration | sí (completa) | función lista | funciona |
| `let` | sí | uninitialized (TDZ) | `ReferenceError` |
| `const` | sí | uninitialized (TDZ) | `ReferenceError` |
| `class` | sí | uninitialized (TDZ) | `ReferenceError` |
| `function` expression (`var`) | sí (solo var) | `undefined` | `undefined` (no es función) |
| `function` expression (`let`) | sí (en TDZ) | uninitialized | `ReferenceError` |

## El orden real de ejecución del motor

El motor JS ejecuta cada contexto de ejecución en dos fases:

**Fase 1 — Creación del contexto:**
El motor recorre el código fuente del ámbito, registra todas las declaraciones en el *Environment Record* y les asigna su estado inicial (`undefined` para `var`; *uninitialized* para `let`/`const`/`class`; la función completa para `function` declarations).

**Fase 2 — Ejecución línea a línea:**
Las instrucciones se ejecutan en el orden escrito. Las asignaciones escriben los valores en los bindings registrados en la fase 1.

```js
// Código fuente:
var a = 1;
let b = 2;
function f() { return 3; }
const c = 4;

// Estado al inicio de la fase de ejecución:
// a → undefined
// b → uninitialized (TDZ)
// f → function f() { return 3; }
// c → uninitialized (TDZ)
```

## Ejemplo práctico — hoisting de función vs. expresión

```js
// Declaration: disponible en todo el ámbito
procesarDatos([1, 2, 3]);

function procesarDatos(arr) {
  return arr.map(x => x * 2);
}

// Expression: solo disponible a partir de su línea
formatear([1, 2, 3]); // TypeError: formatear is not a function

var formatear = function(arr) {
  return arr.join(", ");
};
```

> [!warning] Errores comunes
> **Asumir que `var` no está elevada:** escribir código que "lee" una variable `var` antes de asignarla y esperar `ReferenceError`. El resultado es `undefined`, un bug silencioso.
> **Confundir function declaration con function expression:** las expressions no se elevan completas, solo la variable. Invocarlas antes de la asignación da `TypeError` (si `var`) o `ReferenceError` (si `let`).
> **Declaraciones duplicadas con `var`:** si dos `var` con el mismo nombre se elevan en la misma función, la segunda declaración no crea un nuevo binding; actúa como si la declaración ya existiera (solo importa la asignación).

> [!tip] Buenas prácticas
> Declarar las variables al inicio del ámbito donde se usan — no para "ayudar al hoisting" sino para hacer explícita la dependencia. Preferir `let`/`const` para recibir un error descriptivo (`ReferenceError`) si se comete el error de usar antes de declarar, en lugar del silencioso `undefined` de `var`.

## Notas relacionadas

- [[01 var]]
- [[02 let]]
- [[03 const]]
- [[05 Temporal Dead Zone]]
- [[06 Ámbito (scope)/index|Ámbito (scope)]]
