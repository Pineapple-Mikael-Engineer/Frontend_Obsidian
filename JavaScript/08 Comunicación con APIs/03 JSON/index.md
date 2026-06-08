---
title: JSON — formato de intercambio de datos en APIs web
aliases:
  - JSON
  - JavaScript Object Notation
tags:
  - javascript
  - api/concepto
  - datos
objeto: JSON
tipo: concepto
draft: false
---

# JSON

> [!definicion]
> **JSON (JavaScript Object Notation)** es un formato de texto para intercambio de datos, derivado de la sintaxis de objetos y arrays de JavaScript pero definido como estándar independiente (RFC 8259). Es el formato más extendido en APIs REST. El objeto global `JSON` expone dos métodos: `JSON.stringify()` para serializar valores JS a texto JSON, y `JSON.parse()` para deserializar texto JSON a valores JS.

```js
// Serializar
const texto = JSON.stringify({ id: 1, activo: true, nombre: 'Ana' });
// '{"id":1,"activo":true,"nombre":"Ana"}'

// Deserializar
const obj = JSON.parse('{"id":1,"activo":true}');
// { id: 1, activo: true }
```

## Notas de esta sección

- [[01 Sintaxis JSON|Sintaxis JSON]] — tipos permitidos, tipos prohibidos, diferencias con objeto JS literal.
- [[02 JSON.stringify (replacer, space)|JSON.stringify (replacer, space)]] — serialización, parámetro `replacer` (whitelist / función filtro), indentación, valores especiales, `toJSON()`.
- [[03 JSON.parse (reviver)|JSON.parse (reviver)]] — deserialización, `reviver` para transformar valores (fechas, custom types), deep clone con limitaciones, `structuredClone`.

## Tipos JSON

| Tipo JSON | Ejemplo |
|---|---|
| string | `"hola"` (comillas dobles obligatorias) |
| number | `42`, `3.14`, `-7` |
| boolean | `true`, `false` |
| null | `null` |
| array | `[1, "dos", false]` |
| object | `{"clave": "valor"}` |

Los tipos JS sin equivalente JSON (`undefined`, funciones, `Date`, `Map`, `Set`, `Symbol`, `Infinity`, `NaN`) se omiten o transforman durante `JSON.stringify()`.

## Flujo típico en una API REST

```js
// Enviar datos (JS → JSON)
await fetch('/api/pedidos', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ producto: 'libro', cantidad: 2 })
});

// Recibir datos (JSON → JS)
const res = await fetch('/api/pedidos/42');
const pedido = await res.json();  // res.json() llama JSON.parse internamente
```

## Notas relacionadas

- [[01 Fetch API/index|Fetch API]] — el contexto donde JSON se serializa/deserializa habitualmente
- [[02 XMLHttpRequest (legado)/index|XMLHttpRequest]] — alternativa legada donde JSON se maneja a mano
