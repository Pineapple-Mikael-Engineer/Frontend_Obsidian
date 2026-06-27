---
title: Return — Terminar la función y devolver un valor
aliases:
  - return statement
  - valor de retorno
  - return JS
tags:
  - javascript
  - api/concepto
  - funciones
objeto: global
tipo: concepto
draft: false
order: 7
---

# Return

> [!definicion]
> La instrucción `return expresión` termina la ejecución de la función y devuelve el valor de `expresión` al llamador. `return` sin expresión devuelve `undefined`. Toda función devuelve un valor; si no hay `return` explícito, el valor de retorno es `undefined` implícitamente.

```js
function suma(a, b) {
  return a + b; // termina aquí y devuelve el resultado
  console.log("nunca se ejecuta"); // código muerto
}

function sinReturn(x) {
  x * 2; // expresión evaluada pero descartada
}

suma(3, 4);       // → 7
sinReturn(5);     // → undefined
```

## `return` sin valor

```js
function validar(n) {
  if (typeof n !== "number") return; // return sin valor → undefined
  if (n < 0) return;
  return Math.sqrt(n); // retorno útil
}

validar("texto"); // → undefined
validar(-1);      // → undefined
validar(9);       // → 3
```

`return` sin valor se usa habitualmente para salida anticipada de funciones void (funciones cuyo efecto es un side-effect, no un valor calculado).

## Early return / Guard clauses

Retornar anticipadamente ante condiciones especiales o errores antes del flujo principal. Elimina el else anidado y aplana el código:

```js
// SIN early return — anidamiento creciente
function procesarPedido(pedido) {
  if (pedido) {
    if (pedido.items.length > 0) {
      if (pedido.usuario) {
        // lógica principal aquí adentro
        return calcularTotal(pedido);
      }
    }
  }
}

// CON early return — flujo lineal
function procesarPedido(pedido) {
  if (!pedido) return null;
  if (pedido.items.length === 0) return null;
  if (!pedido.usuario) return null;
  return calcularTotal(pedido); // flujo feliz siempre llega aquí
}
```

## Devolver múltiples valores

JavaScript no admite múltiples valores de retorno directamente. El patrón estándar es devolver un array o un objeto y desestructurar en el llamador:

```js
// Array — por posición (más conciso)
function minMax(arr) {
  return [Math.min(...arr), Math.max(...arr)];
}
const [min, max] = minMax([3, 1, 4, 1, 5, 9]);
// min = 1, max = 9

// Objeto — por nombre (más legible cuando hay más de dos valores)
function dividir(dividendo, divisor) {
  return {
    cociente: Math.trunc(dividendo / divisor),
    resto: dividendo % divisor
  };
}
const { cociente, resto } = dividir(17, 5);
// cociente = 3, resto = 2
```

## Patrón resultado-error (sin excepciones)

Inspirado en Go y Rust: devolver un objeto `{ valor, error }` permite manejar errores sin try/catch:

```js
async function buscarUsuario(id) {
  try {
    const user = await db.find(id);
    return { valor: user, error: null };
  } catch (err) {
    return { valor: null, error: err.message };
  }
}

const { valor, error } = await buscarUsuario(42);
if (error) {
  console.error("Fallo:", error);
} else {
  renderizar(valor);
}
```

## `return` en constructores

En una función constructora invocada con `new`, el comportamiento de `return` es especial:

- Si se devuelve un **objeto**, ese objeto reemplaza a la instancia creada por `new`.
- Si se devuelve un **primitivo** (o nada), el retorno se ignora y `new` devuelve la instancia (`this`).

```js
function Constructor() {
  this.x = 1;
  return { y: 2 }; // objeto → reemplaza la instancia
}
new Constructor(); // → { y: 2 }, NO { x: 1 }

function Constructor2() {
  this.x = 1;
  return 42; // primitivo → ignorado
}
new Constructor2(); // → { x: 1 }
```

## La trampa de ASI con `return`

El mecanismo de **Automatic Semicolon Insertion** inserta automáticamente un `;` después de `return` si va seguido de un salto de línea. El resultado es que la función devuelve `undefined` en lugar del valor en la siguiente línea:

```js
// TRAMPA — el objeto nunca se devuelve
function obtenerConfig() {
  return     // ← ASI inserta ; aquí → return undefined
  {          // este bloque se interpreta como bloque vacío, no como objeto literal
    host: "localhost"
  };
}
obtenerConfig(); // → undefined

// CORRECTO — llave de apertura en la misma línea que return
function obtenerConfig() {
  return {
    host: "localhost"
  };
}
obtenerConfig(); // → { host: "localhost" }
```

Esta es la razón principal por la que en JS se coloca la llave de apertura `{` al final de la línea, no al inicio de la siguiente (a diferencia de Java/C#).

## Cómo funciona por dentro

Cuando el motor ejecuta `return valor`:

1. Evalúa la expresión `valor` en el contexto actual.
2. Escribe ese valor en el **registro de retorno** del frame de ejecución actual.
3. Desapila el frame de la call stack.
4. El frame del llamador lee el registro de retorno y lo usa como resultado de la expresión de llamada.

En ausencia de `return`, el motor alcanza el final del cuerpo y escribe `undefined` en el registro de retorno antes de desapilar.

> [!tip] Buenas prácticas
> - Usar early return (guard clauses) para manejar casos de error al inicio de la función; el flujo feliz queda al final sin anidamiento.
> - Mantener un único tipo de retorno por rama de código (no mezclar retornar un string en algunos casos y un objeto en otros — dificulta el tipado y el razonamiento).
> - Abrir la llave del objeto literal en la misma línea que `return` para evitar la trampa de ASI.

> [!warning] Errores comunes
> **ASI con objeto literal** — el más silencioso:
> ```js
> return
>   { dato: 1 }; // → undefined, no { dato: 1 }
> ```
> **Código muerto después de return** — el linter lo detecta, pero puede indicar lógica errónea:
> ```js
> function f() {
>   return 42;
>   console.log("nunca"); // código muerto
> }
> ```
> **Olvidar el `return` en un callback de `Array.map`** — la función devuelve `undefined` por cada elemento:
> ```js
> const dobles = [1, 2, 3].map(n => {
>   n * 2; // falta el return → undefined
> });
> // dobles = [undefined, undefined, undefined]
> // correcto: n => n * 2  o  n => { return n * 2; }
> ```

## Notas relacionadas

- [[07 Funciones (básico)/index | Funciones (básico)]]
- [[01 Declaración de Función]]
- [[08 Funciones de Primera Clase]]
- [[JavaScript/01 Programación Procedimental/01 Sintaxis Básica/02 Punto y Coma | Punto y Coma — ASI]]
- [[JavaScript/11 Manejo de Errores y Depuración/02 try / catch / finally/index | try / catch / finally]]
