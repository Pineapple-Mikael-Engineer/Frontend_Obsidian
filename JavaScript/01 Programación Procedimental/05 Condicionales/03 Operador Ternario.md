---
title: Operador Ternario — Expresión condicional
aliases:
  - ternario
  - conditional expression
  - ternary operator
  - "?:"
tags:
  - javascript
  - api/concepto
  - control-flujo
objeto: global
tipo: operador
retorna: any
muta: false
asincrono: false
draft: false
---

# Operador Ternario `?:`

> [!definicion]
> El operador ternario `condición ? expresiónSiTrue : expresiónSiFalse` es el único operador ternario de JavaScript. Evalúa `condición` como booleano y retorna el valor de uno de los dos brazos — el izquierdo si la condición es truthy, el derecho si es falsy. Es una **expresión** (produce un valor), no una declaración.

```js
const resultado = condición ? valorSiTrue : valorSiFalse;
```

```js
const edad = 20;
const acceso = edad >= 18 ? "permitido" : "denegado";
// → "permitido"
```

## Expresión vs declaración — por qué importa

`if` es una **declaración**: ocupa una posición de sentencia, no produce valor, no puede usarse donde se espera una expresión.

El ternario es una **expresión**: puede aparecer en cualquier contexto que admita un valor.

```js
// if no puede usarse aquí — error de sintaxis
const label = if (activo) "Activo" else "Inactivo"; // SyntaxError

// El ternario sí puede
const label = activo ? "Activo" : "Inactivo";         // OK
console.log(activo ? "Activo" : "Inactivo");           // OK como argumento
const msg = `Estado: ${activo ? "ON" : "OFF"}`;        // OK en template literal
```

Esta diferencia es la razón de ser del ternario: cubrir los contextos donde `if` no puede entrar.

## Usos canónicos

```js
// 1. Asignación condicional
const precio = esVip ? precioVip : precioBase;

// 2. Argumento condicional a función
registrar(nivel >= 2 ? "verbose" : "normal");

// 3. Valor en template literal
const saludo = `Hola, ${nombre || "usuario"}. Eres ${esMayorEdad ? "mayor" : "menor"} de edad.`;

// 4. Clase CSS condicional (patrón React/Vue/Svelte)
const clases = `btn ${activo ? "btn--activo" : "btn--inactivo"}`;

// 5. Valor de retorno en función de una línea
const abs = (n) => n >= 0 ? n : -n;

// 6. Propiedad condicional en objeto literal
const config = {
  modo: debug ? "development" : "production",
  nivel: verbose ? 3 : 1,
};
```

## Anidamiento — cuándo es demasiado

El ternario puede anidarse, pero más de un nivel se vuelve ilegible:

```js
// Un nivel — aceptable
const tipo = n > 0 ? "positivo" : "no positivo";

// Dos niveles — límite de legibilidad
const tipo = n > 0 ? "positivo" : n < 0 ? "negativo" : "cero";

// Tres niveles — ilegible, usar if/else
const categoria = edad < 13
  ? "niño"
  : edad < 18
    ? "adolescente"
    : edad < 65
      ? "adulto"
      : "adulto mayor";
```

La regla práctica: si el ternario no cabe limpiamente en una línea o requiere más de un nivel de anidamiento, sustituir por `if/else`. El ternario anidado no es más conciso — es más opaco.

> [!warning] Ternario para efectos secundarios
> Usar el ternario para efectos secundarios es posible pero semánticamente incorrecto: el ternario existe para producir un valor, no para controlar flujo con efectos.
>
> ```js
> // Posible pero confuso
> esAdmin ? eliminarUsuario(id) : registrarIntento(id);
>
> // Correcto — usar if/else cuando el valor no se usa
> if (esAdmin) {
>   eliminarUsuario(id);
> } else {
>   registrarIntento(id);
> }
> ```
>
> Si el valor del ternario no se asigna, pasa como argumento, ni se retorna — hay un `if/else` en su lugar.

## Diferencia con `&&` y `||`

Los tres evalúan condiciones y producen valores, pero con semánticas distintas:

| Operador | Rama true | Rama false | Retorna |
|----------|-----------|------------|---------|
| `a ? b : c` | `b` | `c` | Siempre uno de los dos brazos |
| `a && b` | `b` | `a` (el valor falsy) | El primer falsy o el último valor |
| `a \|\| b` | `a` (el valor truthy) | `b` | El primer truthy o el último valor |

```js
const a = null;
const b = "hola";

a ? b : "por defecto"  // → "por defecto"  (ternario: rama false)
a && b                 // → null           (&& retorna a, que es falsy)
a || b                 // → "hola"         (|| retorna el primer truthy)

// La diferencia es crítica cuando el valor truthy es 0 o ""
const n = 0;
n ? n : 10    // → 10   (ternario detecta falsy correctamente)
n || 10       // → 10   (|| también — útil para defaults, pero cambia 0 a 10)
n ?? 10       // → 0    (?? solo reemplaza null/undefined, no 0 ni "")
```

Usar `??` ([[04 Operadores/07 Nullish Coalescing (??)|Nullish Coalescing]]) cuando el default solo debe aplicarse para `null`/`undefined`, no para `0` o `""`.

## Cómo funciona por dentro (spec y motor)

En la gramática ECMAScript, el ternario se define como `ConditionalExpression`:

```
ConditionalExpression:
    ShortCircuitExpression
    ShortCircuitExpression ? AssignmentExpression : AssignmentExpression
```

El motor:

1. Evalúa `ShortCircuitExpression` (la condición).
2. Aplica `ToBoolean` al resultado.
3. Si es `true`, evalúa y retorna el `AssignmentExpression` izquierdo.
4. Si es `false`, evalúa y retorna el `AssignmentExpression` derecho.

El brazo no elegido **no se evalúa** — es evaluación lazy (cortocircuito). Esto es relevante cuando los brazos tienen efectos secundarios o son expresiones costosas:

```js
let n = 0;
true ? n++ : n--;  // solo el brazo izquierdo se ejecuta
console.log(n);    // → 1 (n-- no se ejecutó)

// Relevante con funciones costosas
const dato = cacheHit ? cache.get(key) : calcularDatoLento(key);
// calcularDatoLento solo se invoca si cacheHit es falsy
```

> [!tip] Buenas prácticas
> - Usar el ternario exclusivamente cuando se necesita un **valor** — asignación, argumento, retorno, interpolación.
> - Si el valor producido no se usa, sustituir por `if/else`.
> - Máximo un nivel de anidamiento; a partir del segundo, refactorizar con `if/else` o una función auxiliar.
> - Preferir `??` sobre `||` para valores por defecto cuando `0`, `false` o `""` son valores válidos.
> - En JSX, el ternario es la forma idiomática de renderizado condicional: `{condicion ? <A /> : <B />}`.

> [!warning] Errores comunes
> - **Ternario con efectos secundarios**: `x > 0 ? hacerA() : hacerB()` — semánticamente incorrecto aunque funcione. Usar `if/else`.
> - **Confundir con `||` para defaults**: `valor || "default"` falla si `valor` es `0` o `""`. Usar `valor ?? "default"`.
> - **Anidamiento excesivo**: tres o más niveles de ternario son ilegibles para cualquier lector, incluido el autor original semanas después.
> - **Ternario en el lado izquierdo de asignación**: `(cond ? a : b) = valor` — error de sintaxis. Las expresiones ternarias no son lvalues.

## Notas relacionadas

- [[05 Condicionales/index|Condicionales]] — comparativa de los tres mecanismos.
- [[01 if, else if, else]] — versión declaración, para lógica compleja o efectos secundarios.
- [[02 switch]] — para igualdad sobre valores discretos con muchas ramas.
- [[04 Operadores/07 Nullish Coalescing (??)|Nullish Coalescing (??)]] — default solo para `null`/`undefined`.
- [[04 Operadores/04 Lógicos|Lógicos]] — `&&` y `||` como formas de cortocircuito unilateral.
