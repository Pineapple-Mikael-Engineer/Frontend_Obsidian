---
title: Expresión de Función — Valor en tiempo de ejecución
aliases:
  - function expression
  - expresión de función
  - función anónima
tags:
  - javascript
  - api/concepto
  - funciones
objeto: global
tipo: concepto
draft: false
order: 2
---

# Expresión de Función

> [!definicion]
> Una **expresión de función** es una función que aparece en posición de expresión (lado derecho de una asignación, argumento, operando). El objeto `Function` se crea en tiempo de ejecución cuando el motor alcanza esa línea — no durante el parsing. El nombre es opcional: si se omite, la función es **anónima**.

```js
// Expresión nombrada
const cuadrado = function cuadrado(n) {
  return n * n;
};

// Expresión anónima (nombre inferido del binding)
const doble = function(x) {
  return x * 2;
};

cuadrado(4); // → 16
doble(7);    // → 14
```

## Hoisting: diferencia clave con la declaración

El valor de función **no se hoistea**. Solo la variable se hoistea (si es `var`, con valor `undefined`); con `let`/`const`, ni eso:

```js
// Con var — la variable existe pero el valor aún no
console.log(typeof calcular); // → "undefined" (var hoisted, valor no)
var calcular = function(x) { return x * 3; };
calcular(2); // → 6

// Con const — TDZ hasta la línea de asignación
console.log(calcular2); // → ReferenceError
const calcular2 = function(x) { return x * 3; };
```

## Función nombrada vs anónima

El nombre en una expresión de función tiene dos efectos:

1. **Recursión interna**: el nombre es accesible dentro del cuerpo de la función, lo que permite recursión sin depender de una variable externa.
2. **Stack traces legibles**: el nombre aparece en el error stack en lugar de `<anonymous>`.

```js
// Sin nombre — recursión depende de la variable externa
const factorialV1 = function(n) {
  return n <= 1 ? 1 : n * factorialV1(n - 1); // frágil: si factorialV1 cambia, falla
};

// Con nombre — la referencia interna es estable
const factorialV2 = function factorial(n) {
  return n <= 1 ? 1 : n * factorial(n - 1); // "factorial" es solo visible aquí
};

factorialV2(5); // → 120
typeof factorial; // → "undefined" — el nombre NO existe en el scope externo
```

## Nombre inferido

Cuando una expresión de función anónima se asigna a una variable o propiedad, el motor **infiere** el nombre del binding. La propiedad `.name` refleja esto:

```js
const multiplicar = function(a, b) { return a * b; };
multiplicar.name; // → "multiplicar" (inferido)

const obj = {
  dividir: function(a, b) { return a / b; }
};
obj.dividir.name; // → "dividir" (inferido de la clave)
```

La inferencia es solo cosmética (mejora stack traces); no crea una variable con ese nombre.

## Arrow function como expresión de función

Las arrow functions son un subtipo de expresión de función con tres diferencias estructurales:

```js
// Arrow function — forma compacta
const triple = x => x * 3;

// Arrow con cuerpo completo
const clamp = (valor, min, max) => {
  if (valor < min) return min;
  if (valor > max) return max;
  return valor;
};
```

| Aspecto | Expresión de función | Arrow function |
|---|---|---|
| `this` | propio (determinado en llamada) | léxico (del scope envolvente) |
| `arguments` | sí | no (hereda del externo) |
| `new` | puede ser constructora | no puede |
| Sintaxis cuerpo conciso | no | sí (`=> expresión`) |

Las arrow functions se desarrollan en [[JavaScript/03 Programación Funcional/03 Arrow Functions/index | Arrow Functions]].

## IIFE — Expresión invocada inmediatamente

Una expresión de función puede invocarse en el mismo instante en que se crea, envolviendo la función entre paréntesis (para que el parser la trate como expresión, no como declaración):

```js
// IIFE — crea un ámbito privado
const contador = (function() {
  let count = 0;
  return {
    incrementar() { count++; },
    valor()       { return count; }
  };
})();

contador.incrementar();
contador.incrementar();
contador.valor(); // → 2
// count no es accesible desde fuera
```

El patrón IIFE fue la forma principal de encapsulación antes de los módulos ES6. Hoy se prefieren módulos, pero el patrón sigue siendo relevante en código legacy.

## Recetas comunes

**Callback en addEventListener**:

```js
document.addEventListener("click", function(event) {
  console.log("Clic en:", event.target);
});
```

**Asignación condicional de función** — no posible con declaración:

```js
const formatear = process.env.NODE_ENV === "production"
  ? function(msg) { return msg; }
  : function(msg) { return `[DEBUG] ${msg}`; };
```

**Expresión de función con nombre para recursión segura**:

```js
const fibonacci = function fib(n) {
  if (n <= 1) return n;
  return fib(n - 1) + fib(n - 2);
};

fibonacci(10); // → 55
```

## Cómo funciona por dentro

Cuando el motor ejecuta la línea `const f = function() {}`:

1. Evalúa el lado derecho como una expresión.
2. Crea un nuevo objeto `Function` en el heap, capturando el entorno léxico actual en su slot interno `[[Environment]]`.
3. Asigna la referencia al objeto al binding `f` en el scope actual.

El nombre opcional de la expresión (el identificador entre `function` y `(`) se almacena en un registro de entorno intermedio accesible solo desde dentro del cuerpo. No se filtra al scope externo.

> [!tip] Buenas prácticas
> - Preferir expresiones de función nombradas sobre anónimas para facilitar la depuración: el nombre aparece en el stack trace.
> - Usar expresiones de función (o arrow) para callbacks y funciones que se asignan condicionalmente.
> - Usar declaraciones de función para lógica de módulo que necesita ser visible desde el inicio del scope.

> [!warning] Errores comunes
> **Llamar antes de la asignación** — el error más frecuente al venir de lenguajes con hoisting completo:
> ```js
> procesar(5); // TypeError: procesar is not a function (var) o ReferenceError (const/let)
> const procesar = function(x) { return x * 10; };
> ```
> **Asumir que el nombre interno es global** — el nombre de una expresión nombrada solo existe dentro del cuerpo:
> ```js
> const f = function miNombre() {};
> typeof miNombre; // → "undefined" — no existe fuera
> ```

## Notas relacionadas

- [[07 Funciones (básico)/index | Funciones (básico)]]
- [[01 Declaración de Función]]
- [[JavaScript/01 Programación Procedimental/02 Variables/04 Hoisting | Hoisting]]
- [[JavaScript/03 Programación Funcional/03 Arrow Functions/index | Arrow Functions]]
- [[JavaScript/10 Módulos y Organización/02 Module Pattern (IIFE)/01 IIFE | IIFE]]
