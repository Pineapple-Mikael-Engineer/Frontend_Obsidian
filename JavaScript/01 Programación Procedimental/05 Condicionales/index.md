---
title: Condicionales — Control de flujo por condición
aliases:
  - control condicional
  - branching
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
order: 5
---

# Condicionales

> [!definicion]
> Los condicionales son construcciones de control de flujo que ejecutan bloques de código según el resultado booleano de una expresión. JavaScript dispone de tres mecanismos: `if/else if/else` para condiciones arbitrarias, `switch` para igualdad estricta sobre un valor discreto, y el operador ternario `?:` para producir un valor en función de una condición.

La elección entre los tres no es arbitraria: cada uno encaja en un caso de uso distinto y mezclarlos fuera de su dominio produce código difícil de mantener.

## Cuándo usar cada uno

| Mecanismo | Úsalo cuando... | No lo uses si... |
|-----------|-----------------|------------------|
| `if / else if / else` | La condición involucra rangos, múltiples variables o lógica compleja | La condición es solo igualdad sobre un valor discreto repetido |
| `switch` | Compares el mismo valor con muchos literales (string, number) | Necesites rangos, condiciones boolenas compuestas, o coerción |
| Ternario `?:` | Necesitas un valor en una expresión (asignación, argumento, template) | La rama tiene efectos secundarios complejos o más de una instrucción |

## Los tres mecanismos

Las hojas de esta sección desarrollan cada uno en profundidad:

- [[01 if, else if, else]] — sintaxis, truthy/falsy, guard clauses, anidamiento, early return.
- [[02 switch]] — fall-through, `break`, `default`, comparación `===`, tabla de dispatch.
- [[03 Operador Ternario]] — expresión vs declaración, usos en JSX/templates, anidamiento, diferencia con `&&`/`||`.

## Modelo mental

Los tres mecanismos comparten la misma base: el motor convierte la condición a booleano mediante `ToBoolean` (ver [[03 Tipos de Datos/04 Conversión de Tipos/03 Truthy y Falsy|Truthy y Falsy]]) y ejecuta la rama correspondiente. La diferencia es **sintáctica y semántica**:

- `if/else` es una **declaración** — admite cualquier bloque de sentencias.
- `switch` es una **declaración** — compara con `===` y puede caer al siguiente `case` si no hay `break`.
- El ternario `?:` es una **expresión** — produce un valor; los dos brazos son expresiones, no bloques.

```js
// if/else — rama con lógica compleja
function clasificar(n) {
  if (n < 0) return "negativo";
  if (n === 0) return "cero";
  if (n < 100) return "pequeño";
  return "grande";
}

// switch — igualdad sobre valor discreto
function emoji(estado) {
  switch (estado) {
    case "ok":      return "✅";
    case "error":   return "❌";
    case "pending": return "⏳";
    default:        return "❓";
  }
}

// ternario — valor en expresión
const label = activo ? "Activo" : "Inactivo";
```

## Tabla comparativa

| Aspecto | `if/else` | `switch` | Ternario `?:` |
|---------|-----------|----------|---------------|
| Tipo sintáctico | Declaración | Declaración | Expresión |
| Condición | Cualquier booleana | Igualdad `===` sobre una expr | Cualquier booleana |
| Número de ramas | Ilimitado | Ilimitado (`case`) | Exactamente 2 |
| Produce valor | No | No | Sí |
| Fall-through posible | No | Sí (sin `break`) | No |
| Legibilidad con muchos casos | Baja | Alta | Muy baja |
| Uso en JSX / template literal | No | No | Sí |

## Notas relacionadas

- [[03 Tipos de Datos/04 Conversión de Tipos/03 Truthy y Falsy|Truthy y Falsy]] — qué valores son falsos en JavaScript.
- [[03 Tipos de Datos/04 Conversión de Tipos/04 Igualdad (== vs ===)|Igualdad (== vs ===)]] — por qué `switch` usa `===`.
- [[04 Operadores/index|Operadores]] — operadores lógicos `&&`, `||`, `??` como alternativa condicional.
- [[06 Bucles/index|Bucles]] — control de flujo iterativo.
