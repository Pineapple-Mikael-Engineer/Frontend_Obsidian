---
title: if / else if / else — Condicional de propósito general
aliases:
  - if else
  - sentencia if
  - if statement
tags:
  - javascript
  - api/concepto
  - control-flujo
objeto: global
tipo: concepto
retorna: undefined
muta: false
asincrono: false
draft: false
---

# `if / else if / else`

> [!definicion]
> `if` evalúa una expresión como booleano y ejecuta el bloque `then` si el resultado es `true`. `else if` encadena condiciones adicionales que se evalúan solo si las anteriores fueron falsas. `else` captura el caso restante. Los tres son declaraciones — no producen valor.

```js
if (condición) {
  // se ejecuta si condición es truthy
} else if (otraCondición) {
  // se ejecuta si la primera es falsy y esta es truthy
} else {
  // se ejecuta si todas las anteriores son falsas
}
```

## Sintaxis con y sin llaves

Las llaves son opcionales cuando el cuerpo es una sola sentencia, pero **omitirlas es un antipatrón** que produce dos bugs clásicos:

```js
// SIN llaves — funcionalmente correcto, pero frágil
if (activo) console.log("activo");

// Bug del dangling else: el else pertenece al if más cercano
if (x > 0)
  if (x < 10)
    console.log("entre 1 y 9");
  else
    console.log("esto es del if(x<10), no del if(x>0)"); // trampa

// Bug de la línea añadida: parece que la segunda línea está en el if
if (activo)
  console.log("activo");
  ejecutarAccion(); // se ejecuta siempre, no solo si activo
```

```js
// CON llaves — nunca hay ambigüedad
if (activo) {
  console.log("activo");
  ejecutarAccion();
}
```

> [!warning] Siempre usar llaves
> La omisión de llaves es fuente frecuente de bugs silenciosos. Linters como ESLint (`curly: "all"`) la prohíben en proyectos serios. La única excepción aceptada es el early return de una sola línea en funciones cortas.

## La condición es cualquier expresión — ToBoolean

El motor convierte la condición a booleano mediante la operación abstracta `ToBoolean`. Cualquier valor puede ser condición; los **valores falsy** en JavaScript son exactamente siete:

| Valor | Tipo |
|-------|------|
| `false` | boolean |
| `0` | number |
| `-0` | number |
| `0n` | bigint |
| `""` | string |
| `null` | object |
| `undefined` | undefined |
| `NaN` | number |

Todo lo demás — incluyendo `[]`, `{}`, `"0"`, `" "` — es **truthy**.

```js
if ([]) console.log("truthy");  // → truthy (array vacío es objeto)
if ("") console.log("truthy");  // no se ejecuta (string vacío es falsy)
if (0)  console.log("truthy");  // no se ejecuta

// Trampa frecuente: comparar con booleano explícito
const items = [];
if (items.length) { /* solo entra si hay elementos */ }
// NO hacer: if (items.length == true) — coerción inesperada
```

## `else if` vs `else { if }`

`else if` no es una palabra clave propia — es la composición de `else` seguido de un `if` sin llaves. Ambas formas son semánticamente equivalentes; la primera es canónica:

```js
// Forma canónica (preferida)
if (n < 0) {
  return "negativo";
} else if (n === 0) {
  return "cero";
} else {
  return "positivo";
}

// Forma equivalente (más anidada visualmente, evitar)
if (n < 0) {
  return "negativo";
} else {
  if (n === 0) {
    return "cero";
  } else {
    return "positivo";
  }
}
```

## Anidamiento — cuándo es demasiado

Más de dos niveles de `if` anidados indica que la función está haciendo demasiado o que falta una extracción. El umbral práctico es **dos niveles de profundidad**; a partir del tercero, refactorizar.

```js
// Mal — tres niveles de anidamiento
function procesar(usuario) {
  if (usuario) {
    if (usuario.activo) {
      if (usuario.permisos.includes("admin")) {
        realizarAccion();
      }
    }
  }
}

// Bien — guard clauses (ver sección siguiente)
function procesar(usuario) {
  if (!usuario) return;
  if (!usuario.activo) return;
  if (!usuario.permisos.includes("admin")) return;
  realizarAccion();
}
```

## Early return / Guard clauses

El patrón de retorno temprano invierte las condiciones de error: en lugar de anidar la lógica principal dentro de `if (condicionOk)`, se valida primero cada caso límite y se retorna (o lanza) inmediatamente. El cuerpo principal queda sin anidar al final.

```js
// Sin guard clauses — lógica principal enterrada en anidamiento
function dividir(a, b) {
  if (typeof a === "number" && typeof b === "number") {
    if (b !== 0) {
      return a / b;
    } else {
      throw new Error("División por cero");
    }
  } else {
    throw new TypeError("Argumentos deben ser números");
  }
}

// Con guard clauses — limpio, lineal
function dividir(a, b) {
  if (typeof a !== "number" || typeof b !== "number") {
    throw new TypeError("Argumentos deben ser números");
  }
  if (b === 0) throw new Error("División por cero");
  return a / b;
}
```

El patrón es especialmente valioso en funciones de validación, parsers y manejadores de eventos donde los casos de error son frecuentes y variados.

## Recetas comunes

```js
// 1. Asignación condicional con if (cuando hay lógica compleja)
let mensaje;
if (errores.length > 0) {
  mensaje = `${errores.length} errores encontrados`;
} else if (advertencias.length > 0) {
  mensaje = "Completado con advertencias";
} else {
  mensaje = "Éxito";
}

// 2. Validación de formulario con early return
function validarEmail(email) {
  if (!email) return { ok: false, error: "Email requerido" };
  if (!email.includes("@")) return { ok: false, error: "Email inválido" };
  if (email.length > 254) return { ok: false, error: "Email demasiado largo" };
  return { ok: true };
}

// 3. Condición con rango — if gana sobre switch
function categoriaEdad(edad) {
  if (edad < 0) throw new RangeError("Edad inválida");
  if (edad < 13) return "niño";
  if (edad < 18) return "adolescente";
  if (edad < 65) return "adulto";
  return "adulto mayor";
}

// 4. Múltiples condiciones por rama — solo if puede expresarlo
function clasificarTriangulo(a, b, c) {
  if (a === b && b === c) return "equilátero";
  if (a === b || b === c || a === c) return "isósceles";
  return "escaleno";
}
```

## Cuándo preferir `if` sobre `switch`

`if/else if` gana cuando:

- La condición involucra **rangos** (`edad > 65`, `precio < 100`).
- Cada rama tiene **condiciones compuestas** (`x > 0 && y > 0`).
- Las condiciones son **heterogéneas** (distintas variables en cada rama).
- Se necesita verificar la misma expresión con **operadores distintos** a `===`.

Para igualdad simple sobre el mismo valor discreto con muchas ramas, [[02 switch]] es más legible.

## Cómo funciona por dentro (motor)

El parser de V8 (y cualquier motor ES) reconoce `if` como `IfStatement` en la gramática. Al ejecutar:

1. El motor evalúa `Expression` en `if (Expression)`.
2. Aplica `ToBoolean(valor)` al resultado: convierte mediante una tabla de tipos, sin invocar `valueOf` ni `toString` en la mayoría de casos.
3. Si el resultado es `true`, ejecuta `Statement_then`.
4. Si es `false`, evalúa la siguiente rama `else if` (repite desde 1) o ejecuta el bloque `else` si existe.
5. Las ramas no evaluadas no se ejecutan en absoluto — no hay evaluación diferida ni lazy loading de closures; simplemente el flujo de control salta el bloque.

El motor puede optimizar cadenas de `if/else if` con **jump tables** o **inline caches** si detecta patrones de igualdad repetidos sobre el mismo tipo, pero esto es transparente al programador.

> [!tip] Buenas prácticas
> - Siempre llaves, aunque el cuerpo sea una línea.
> - Guard clauses para reducir anidamiento: validar y retornar primero, lógica principal al final.
> - Preferir condiciones positivas (`if (activo)`) sobre negaciones dobles (`if (!inactivo)`).
> - Si la cadena `else if` supera 4-5 ramas de igualdad sobre el mismo valor, considerar `switch` o una tabla de objetos.
> - No comparar booleans con `== true`/`== false`; usar la expresión directamente.

> [!warning] Errores comunes
> - **Dangling else**: sin llaves, el `else` pertenece al `if` más cercano, no al más lejano.
> - **Comparación con `==`**: `if (x == null)` captura `null` y `undefined` (puede ser intencional, pero documentarlo). Usar `===` por defecto.
> - **`NaN` nunca es igual a nada**: `if (x === NaN)` siempre es `false`; usar `Number.isNaN(x)`.
> - **Asignación en condición**: `if (x = 5)` asigna y siempre es truthy — probablemente un error. ESLint `no-cond-assign` lo detecta.
> - **Asumir que `[]` es falsy**: `if ([])` entra en la rama truthy. Comprobar `.length` si se quiere verificar vacuidad.

## Notas relacionadas

- [[05 Condicionales/index|Condicionales]] — comparativa de los tres mecanismos.
- [[02 switch]] — alternativa para igualdad discreta con muchas ramas.
- [[03 Operador Ternario]] — versión expresión para asignaciones condicionales simples.
- [[03 Tipos de Datos/04 Conversión de Tipos/03 Truthy y Falsy|Truthy y Falsy]] — tabla completa de valores falsy.
