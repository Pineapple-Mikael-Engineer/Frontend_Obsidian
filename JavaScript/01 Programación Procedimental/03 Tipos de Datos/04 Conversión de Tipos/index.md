---
title: Conversión de Tipos — Coerción explícita e implícita en JavaScript
aliases:
  - conversión de tipos JS
  - type coercion
  - coercion JavaScript
tags:
  - javascript
  - api/concepto
  - tipos
objeto: global
tipo: concepto
draft: false
---

# Conversión de Tipos

> [!definicion]
> JavaScript convierte tipos de dos formas: **explícita** (el programador llama a `Number()`, `String()`, `Boolean()`) e **implícita** (el motor convierte automáticamente en operadores y contextos). La conversión implícita —también llamada **coerción**— es una fuente frecuente de comportamiento inesperado.

```js
// Explícita: intención clara
Number("42")      // → 42
String(true)      // → "true"
Boolean(0)        // → false

// Implícita: el motor convierte sin que el programador lo indique
"5" * 2           // → 10    (string → number por el operador *)
"5" + 2           // → "52"  (number → string por el operador +)
if ("hola") {}    // → truthy: el string se convierte a true
```

## Métodos de conversión explícita

| Función | Entrada típica | Resultado |
|---|---|---|
| `Number(v)` | string numérico | valor numérico o `NaN` |
| `parseInt(s, base)` | string | entero, parsea hasta invalido |
| `parseFloat(s)` | string | decimal, parsea hasta invalido |
| `String(v)` | cualquier valor | representacion string (segura para null/undefined) |
| `Boolean(v)` | cualquier valor | `true` o `false` segun truthy/falsy |

## Contextos de coerción implícita

| Contexto | Conversión aplicada | Ejemplo |
|---|---|---|
| Operador `+` (un operando string) | ToNumber o ToString | `"3" + 1` → `"31"` |
| Operadores aritméticos `* / - %` | ToNumber | `"6" / "2"` → `3` |
| Operadores relacionales `< > <= >=` | ToNumber o lexicográfico | `"10" > 9` → `true` |
| Condición `if` / `while` / `? :` | ToBoolean | `if (0)` → no entra |
| `==` (igualdad abstracta) | ToNumber / ToString / ToPrimitive | `"1" == 1` → `true` |
| Template literals `${}` | ToString | `` `${true}` `` → `"true"` |

## Subsecciones

- [[01 Conversión Explícita (Number, String, Boolean)|Conversión Explícita]] — `Number()`, `parseInt()`, `String()`, `Boolean()`, `!!`.
- [[02 Conversión Implícita|Conversión Implícita]] — coerción por operadores, `==`, algoritmos internos `ToPrimitive`/`ToNumber`.
- [[03 Truthy y Falsy|Truthy y Falsy]] — los 6+1 valores falsy, guards, `||` vs `??`.
- [[04 Igualdad (== vs ===)|Igualdad == vs ===]] — igualdad estricta, abstracta, `Object.is()`, la regla práctica.

## Los algoritmos internos del spec

El motor usa cuatro operaciones abstractas internas para todas las conversiones:

| Operación | Qué hace |
|---|---|
| `ToBoolean` | Convierte cualquier valor a `true`/`false` según la tabla falsy |
| `ToNumber` | Convierte a número; base de `Number()`, operadores aritméticos |
| `ToString` | Convierte a string; base de `String()`, template literals |
| `ToPrimitive` | Convierte un objeto a primitivo llamando `valueOf()` o `toString()` |

La coerción implícita encadena estas operaciones según las reglas del operador. La nota [[02 Conversión Implícita|Conversión Implícita]] detalla los algoritmos.

> [!tip] Regla práctica
> Usar siempre `===` en lugar de `==` para evitar coerción en comparaciones. La única excepción idiomática aceptada es `valor == null` para comprobar null-o-undefined a la vez.

## Notas relacionadas

- [[01 Conversión Explícita (Number, String, Boolean)|Conversión Explícita]]
- [[02 Conversión Implícita|Conversión Implícita]]
- [[03 Truthy y Falsy|Truthy y Falsy]]
- [[04 Igualdad (== vs ===)|Igualdad == vs ===]]
- [[03 Tipos de Datos/index|Tipos de Datos]]
