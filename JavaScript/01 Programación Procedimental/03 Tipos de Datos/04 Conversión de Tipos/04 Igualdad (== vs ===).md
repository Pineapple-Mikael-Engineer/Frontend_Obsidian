---
title: "Igualdad en JavaScript — == vs ==="
aliases:
  - "== vs ==="
  - igualdad estricta js
  - igualdad abstracta js
tags:
  - javascript
  - api/concepto
  - tipos
draft: false
---

# Igualdad (== vs ===)

> [!definicion]
> JavaScript tiene dos operadores de igualdad: `===` (**estricta**, sin coerción) y `==` (**abstracta**, con coerción de tipos). `===` devuelve `true` solo si los valores tienen el mismo tipo Y el mismo valor. `==` permite que el motor convierta tipos antes de comparar, produciendo resultados contraintuitivos. La regla práctica: usa siempre `===`.

```js
1 === 1       // → true
1 === "1"     // → false  (distinto tipo)
1 == "1"      // → true   (string "1" → número 1)
null == undefined // → true  (caso especial de ==)
null === undefined // → false (distintos tipos)
```

## === Igualdad estricta

Dos valores son estrictamente iguales si:
1. Son del mismo tipo.
2. Tienen el mismo valor.

Sin excepciones de coerción. Con solo dos casos especiales propios:

```js
NaN === NaN     // → false  (NaN no es igual a nada, ni a sí mismo)
+0 === -0       // → true   (cero positivo y negativo son iguales con ===)
```

Para esos dos casos usar `Number.isNaN()` y `Object.is()`:

```js
Number.isNaN(NaN)      // → true
Object.is(NaN, NaN)    // → true
Object.is(+0, -0)      // → false
```

## == Igualdad abstracta — el algoritmo

El algoritmo Abstract Equality Comparison del spec evalúa en este orden:

1. Si `typeof x === typeof y` → comparación estricta (sin coerción más).
2. `null == undefined` → `true` (y `undefined == null` → `true`).
3. `null` o `undefined` comparado con cualquier otra cosa → `false`.
4. Si un operando es `number` y el otro `string` → convierte el string a número.
5. Si un operando es `boolean` → conviértelo a número (`true→1`, `false→0`), luego repite.
6. Si un operando es `object` y el otro es `number`, `string`, `bigint` o `symbol` → `ToPrimitive(objeto)`, luego repite.

### Casos más llamativos de ==

| Expresión | Resultado | Motivo |
|---|---|---|
| `0 == false` | `true` | `false → 0`, luego `0 == 0` |
| `"" == false` | `true` | `false → 0`, `"" → 0`, luego `0 == 0` |
| `"1" == 1` | `true` | `"1" → 1` |
| `null == undefined` | `true` | caso especial |
| `null == 0` | `false` | null solo es igual a undefined |
| `null == false` | `false` | null solo es igual a undefined |
| `[] == false` | `true` | `[] → "" → 0`, `false → 0` |
| `[] == ![]` | `true` | `![] → false → 0`, `[] → 0` |
| `{} == "[object Object]"` | `true` | `{}.toString() → "[object Object]"` |

## Object.is() — igualdad SameValue

`Object.is(a, b)` implementa el algoritmo **SameValue** del spec: igual que `===` pero trata `NaN === NaN` como `true` y `-0 !== +0`.

```js
Object.is(NaN, NaN)     // → true  (a diferencia de ===)
Object.is(+0, -0)       // → false (a diferencia de ===)
Object.is(1, 1)         // → true
Object.is("a", "a")     // → true
Object.is(null, null)   // → true
```

Es la igualdad más precisa del lenguaje. Se usa internamente por `Map` y `Set` para comparar claves, y por `Array.prototype.includes` para encontrar `NaN`.

## Comparación de objetos

Con `==` y `===`, dos objetos son iguales solo si son **la misma referencia en memoria**, independientemente de su contenido:

```js
const a = { x: 1 };
const b = { x: 1 };
a === b   // → false (objetos distintos en memoria)
a == b    // → false (misma regla)
a === a   // → true  (misma referencia)
```

Para comparar contenido, usar JSON, `structuredClone`, o una librería como Lodash (`_.isEqual`).

## La regla práctica

| Cuándo | Usar |
|---|---|
| Siempre que sea posible | `===` |
| Comprobar `null` o `undefined` a la vez | `== null` |
| Comparar valores cuyo tipo ya es conocido | `===` (sigue siendo mejor) |
| Algoritmos que necesitan igualdad precisa de `NaN` o `-0` | `Object.is()` |

La única excepción idiomática ampliamente aceptada para `==` es el patrón `valor == null`:

```js
// Equivale a: valor === null || valor === undefined
// Más conciso y aceptado por la comunidad
if (resultado == null) {
  return "sin resultado";
}
```

## Cómo funciona por dentro

V8 compila `===` a una comparación directa de tipo y valor en el nivel de máquina. `==` requiere ejecutar el algoritmo de coerción antes: puede implicar llamadas a `ToPrimitive`, `ToNumber`, y ramas adicionales. En código crítico, `===` es marginalmente más rápido. En código normal, la diferencia es irrelevante, pero la legibilidad y predecibilidad de `===` son razones suficientes para preferirlo siempre.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `===` por defecto en todo el código.
> - Usa `== null` cuando necesites comprobar null-o-undefined en una sola expresión.
> - Para `NaN`, usa `Number.isNaN()` — nunca `valor === NaN` (siempre false).
> - Para `-0`, usa `Object.is(valor, -0)` si el signo importa.
> - Habilita ESLint con la regla `eqeqeq` para que el linter enforce `===`.

## Errores comunes

> [!warning] Trampas
> - **`NaN === NaN` es `false`**: la única forma segura de comprobar NaN es `Number.isNaN()`.
> - **`== false` para comprobar falsiness**: `0 == false` es `true`, pero `"" == false` también. Usa simplemente `if (!valor)` o `Boolean(valor) === false`.
> - **Comparar objetos con `==` esperando comparar contenido**: compara referencias, no valores.
> - **`[] == false` es `true` pero `Boolean([])` es `true`**: la coerción de `==` y la conversión con `Boolean()` siguen caminos distintos.

## Notas relacionadas

- [[02 Conversión Implícita]] — el mecanismo de coerción que usa `==`.
- [[03 Truthy y Falsy]] — los valores que se convierten a `false`.
- [[03 Comparación]] — los operadores relacionales y `===`/`==` en contexto.
- [[index]] — conversión de tipos en general.
