---
title: Conversión Implícita (Coerción) en JavaScript
aliases:
  - coerción js
  - type coercion javascript
  - conversión implícita js
tags:
  - javascript
  - api/concepto
  - tipos
draft: false
order: 2
---

# Conversión Implícita (Coerción)

> [!definicion]
> La **coerción** es la conversión automática de tipos que el motor JavaScript aplica cuando los operandos de un operador no coinciden en tipo. No requiere ninguna llamada explícita del programador: ocurre silenciosamente. Comprender cuándo y cómo ocurre es clave para evitar bugs sutiles.

```js
// Coerción en acción
"5" - 2      // → 3   (string → number: el operador - convierte)
"5" + 2      // → "52" (number → string: + con string es concatenación)
true + 1     // → 2   (boolean → number)
null + 1     // → 1   (null → 0)
undefined + 1 // → NaN (undefined → NaN)
```

## El operador + y la ambigüedad suma/concatenación

`+` es el operador más sorprendente porque tiene dos significados:

- Si **algún operando es string** → concatenación (ambos se convierten a string).
- Si **ninguno es string** → suma numérica (ambos se convierten a número).

```js
1 + 2          // → 3      (número + número)
"1" + 2        // → "12"   (string + número → concatenación)
1 + "2"        // → "12"   (número + string → concatenación)
1 + 2 + "3"    // → "33"   (izq a dcha: 1+2=3, luego 3+"3"="33")
"1" + 2 + 3    // → "123"  (izq a dcha: "1"+2="12", luego "12"+3="123")
true + true    // → 2      (boolean → number)
[] + []        // → ""     (array.toString() = "")
[] + {}        // → "[object Object]"
{} + []        // → 0      (cuando {} es bloque, no objeto; +[] = 0)
```

## Los algoritmos internos: ToPrimitive, ToNumber, ToString

El spec ECMAScript define operaciones abstractas que el motor aplica internamente:

### ToPrimitive(valor, tipoPreferido)

Convierte un objeto a primitivo. El motor llama a:
1. `[Symbol.toPrimitive](hint)` si existe (hint: `"number"`, `"string"`, o `"default"`).
2. Si no, `valueOf()` — si devuelve primitivo, lo usa.
3. Si no, `toString()` — si devuelve primitivo, lo usa.
4. Si ninguno devuelve primitivo → TypeError.

```js
const obj = {
  valueOf() { return 42; },
  toString() { return "objeto"; }
};
obj + 1   // → 43  (valueOf() gana en contexto numérico)
`${obj}`  // → "objeto"  (toString() gana en contexto string)
```

### ToNumber(valor)

| Valor | Resultado |
|---|---|
| `undefined` | `NaN` |
| `null` | `0` |
| `true` | `1` |
| `false` | `0` |
| `""` | `0` |
| `"42"` | `42` |
| `"42abc"` | `NaN` |
| `[]` | `0` (vía `"" → 0`) |
| `[1]` | `1` (vía `"1" → 1`) |
| `[1,2]` | `NaN` (vía `"1,2" → NaN`) |
| `{}` | `NaN` |

### ToString(valor)

| Valor | Resultado |
|---|---|
| `undefined` | `"undefined"` |
| `null` | `"null"` |
| `true` | `"true"` |
| `false` | `"false"` |
| `0` | `"0"` |
| `-0` | `"0"` (oculta el signo) |
| `Infinity` | `"Infinity"` |
| `NaN` | `"NaN"` |
| `[]` | `""` |
| `[1, 2]` | `"1,2"` |
| `{}` | `"[object Object]"` |

## Coerción en operadores relacionales

`<`, `>`, `<=`, `>=` convierten los operandos a número **salvo** que ambos sean string, en cuyo caso usan comparación lexicográfica Unicode carácter a carácter.

```js
"10" > 9      // → true  (string "10" → número 10, luego 10 > 9)
"10" > "9"    // → false (lexicográfico: "1" < "9" en Unicode)
null > 0      // → false (null → 0, 0 > 0 es false)
null == 0     // → false (== tiene reglas especiales para null)
null >= 0     // → true  (null → 0, 0 >= 0 es true — inconsistente con ==)
```

## Coerción en ==

El operador `==` aplica el algoritmo **Abstract Equality Comparison** que sigue este orden simplificado:

1. Si mismos tipos → comparación estricta (sin coerción).
2. `null == undefined` → `true` (caso especial).
3. Si un operando es `number` y el otro `string` → convierte el string a number.
4. Si un operando es `boolean` → convierte el boolean a number.
5. Si un operando es `object` y el otro es primitivo → `ToPrimitive` al objeto.

```js
null == undefined   // → true  (caso especial, sin coerción)
null == 0           // → false (null no se coerciona a number con ==)
false == 0          // → true  (false → 0)
"" == false         // → true  ("" → 0, false → 0)
"1" == 1            // → true  ("1" → 1)
[1] == 1            // → true  ([1].toString() = "1" → 1)
[] == false         // → true  ([] → "" → 0, false → 0)
```

## Recetas comunes

### Evitar la coerción de + para concatenar

```js
// El problema
const resultado = precio + impuesto; // ¿suma o concatenación?

// Solución: ser explícito
const suma = Number(precio) + Number(impuesto);
const concat = String(precio) + String(impuesto);
```

### La única excepción idiomática de ==

La única excepción donde `==` tiene sentido sobre `===` es comprobar `null` **o** `undefined` a la vez:

```js
// Equivale a: valor === null || valor === undefined
if (valor == null) { /* null o undefined */ }
```

## Cómo funciona por dentro

El motor analiza los tipos de los operandos en tiempo de ejecución y llama a las operaciones abstractas del spec. V8 usa un sistema de especulación de tipos (Turbofan) que asume tipos estables; la coerción obliga al compilador a insertar rutas lentas (slow paths) que degradan el rendimiento. Por eso evitar la coerción no es solo una cuestión de legibilidad: también tiene implicaciones de rendimiento en código crítico.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa siempre `===` en lugar de `==` — la única excepción aceptada es `== null`.
> - Para concatenar strings con números, usa template literals: `` `${precio} €` `` en vez de `precio + " €"`.
> - Para convertir a número, usa `Number()` o el unario `+`; para string, usa `String()` o template literals.
> - Si defines métodos `valueOf()`/`toString()` en objetos, sé consciente de que el motor los usará en coerción implícita.

## Errores comunes

> [!warning] Trampas
> - **Sumar string + número**: `"Total: " + 5 + 10` da `"Total: 510"`, no `"Total: 15"`. Usa `"Total: " + (5 + 10)`.
> - **Confiar en que `null` se convierte a `0` con `==`**: `null == 0` es `false`. `null` solo es igual a `undefined` con `==`.
> - **Comparar strings numéricamente con `>`**: `"10" > "9"` es `false` (lexicográfico). Convierte con `Number()` primero.
> - **`[] == false`**: es `true` — pero `if ([])` entra porque `[]` es truthy como objeto.

## Notas relacionadas

- [[01 Conversión Explícita (Number, String, Boolean)]] — la alternativa explícita y predecible.
- [[03 Truthy y Falsy]] — los valores que se convierten a `false` en contextos booleanos.
- [[04 Igualdad (== vs ===)]] — tabla completa y regla práctica.
- [[index]] — contexto de la conversión de tipos.
