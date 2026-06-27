---
title: JSON.parse — deserializar texto JSON a valor JS
aliases:
  - JSON.parse
  - parse JSON
  - reviver JSON
tags:
  - javascript
  - api/metodo
  - datos
objeto: JSON
tipo: metodo
retorna: any
muta: false
asincrono: false
draft: false
order: 3
---

# `JSON.parse` — deserializar texto JSON

> [!definicion]
> `JSON.parse(texto, reviver?)` convierte un string JSON a su valor JS equivalente. Lanza `SyntaxError` si el string no es JSON válido. El parámetro opcional `reviver` es una función `(key, value) => value` que se aplica de forma bottom-up sobre cada clave del resultado para transformar valores durante el parseo (uso clásico: reconstruir objetos `Date` desde strings ISO 8601).

```js
JSON.parse('{"id":1,"activo":true}');
// { id: 1, activo: true }

JSON.parse('[1, "dos", null]');
// [1, 'dos', null]

JSON.parse('texto inválido');
// SyntaxError: Unexpected token t in JSON at position 0
```

## Firma

```js
JSON.parse(texto)
JSON.parse(texto, reviver)
```

`texto` debe ser un string JSON estrictamente válido (ver [[01 Sintaxis JSON|Sintaxis JSON]]). Cualquier desviación — coma final, clave sin comillas, `undefined` literal — lanza `SyntaxError`.

## Manejo de errores

`JSON.parse` no tiene modo silencioso: lanza o retorna el valor. Envolver en `try/catch` siempre que el JSON provenga de una fuente externa:

```js
function parseSeguro(texto, fallback = null) {
  try {
    return JSON.parse(texto);
  } catch {
    return fallback;
  }
}

parseSeguro('{"ok":true}');  // { ok: true }
parseSeguro('basura');       // null (fallback)
```

## Parámetro `reviver`

Función `(key, value) => transformedValue` llamada **bottom-up** para cada clave (hijos antes que padres). La última llamada tiene `key === ''` y `value` es el objeto raíz completo.

Retornar `undefined` desde el reviver elimina la propiedad del resultado.

### Caso de uso: reconstruir `Date` desde string ISO 8601

`JSON.stringify` serializa `Date` como `"2024-06-08T10:00:00.000Z"`. Al parsear, ese string queda como string JS; hay que reconstruir el objeto `Date` manualmente con el reviver:

```js
const ISO_REGEX = /^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}/;

const obj = JSON.parse(textoJSON, (key, value) =>
  typeof value === 'string' && ISO_REGEX.test(value)
    ? new Date(value)
    : value
);
```

Aplicado sobre `'{"creado":"2024-06-08T10:00:00.000Z","id":1}'`:

```js
// { creado: Date("2024-06-08T10:00:00.000Z"), id: 1 }
// obj.creado instanceof Date → true
```

### Receta: omitir propiedades según criterio

```js
const resultado = JSON.parse(texto, (key, value) => {
  if (key === '') return value;          // raíz: siempre pasar
  if (key === 'password') return undefined;  // eliminar
  return value;
});
```

## `JSON.parse(JSON.stringify(obj))` — deep clone con limitaciones

El patrón `JSON.parse(JSON.stringify(obj))` se usa como deep clone rápido de objetos planos:

```js
const original = { a: 1, b: { c: 2 } };
const clon = JSON.parse(JSON.stringify(original));
clon.b.c = 99;
console.log(original.b.c); // 2 — no afecta al original
```

Limitaciones del patrón:

| Tipo en el original | Resultado en el clon |
|---|---|
| `undefined` (propiedad) | Propiedad omitida |
| `function` | Propiedad omitida |
| `Date` | String ISO (no `Date`) |
| `Map` / `Set` | `{}` / `[]` (contenido perdido) |
| Referencia circular | Lanza `TypeError` |
| `NaN` / `Infinity` | `null` |

## `structuredClone` — deep clone moderno

Para deep clone que preserve tipos especiales, usar `structuredClone(obj)` (disponible en navegadores modernos y Node 17+):

```js
const original = {
  fecha: new Date('2024-01-01'),
  mapa: new Map([[1, 'a']]),
  conjunto: new Set([1, 2, 3])
};

const clon = structuredClone(original);
clon.fecha instanceof Date;  // true
clon.mapa instanceof Map;    // true
clon.conjunto instanceof Set; // true
```

`structuredClone` soporta: `Date`, `Map`, `Set`, `ArrayBuffer`, `TypedArray`, `RegExp`, `Error`. No soporta: funciones ni nodos DOM (lanza `DataCloneError`).

## Cómo funciona por dentro

`JSON.parse` aplica el reviver en **post-order**: primero los valores hoja, luego sus padres, y al final la raíz. Esto permite que la transformación de un hijo esté completa antes de que el padre la vea. El objeto raíz recibe la clave `''` (string vacío) como convención para distinguirlo de propiedades reales.

```js
// Orden de llamadas del reviver para {"a":{"b":1},"c":2}:
// 1. key="b", value=1
// 2. key="a", value={b:1}
// 3. key="c", value=2
// 4. key="",  value={a:{b:1},c:2}  ← raíz
```

> [!tip]
> El reviver de fechas ISO es la razón principal para usar `JSON.parse` con segundo argumento en producción. Si el backend devuelve fechas como strings ISO, un reviver centralizado en el cliente de API evita que la fecha llegue como string a toda la aplicación.

> [!warning]
> `JSON.parse` confía en que `texto` es un string. Si se le pasa `null`, `undefined` o un número, los convierte a string antes (`"null"`, `"undefined"`, `"42"`) y puede retornar `null`, lanzar `SyntaxError` o retornar el número. Validar la entrada antes de parsear cuando proviene de `localStorage`, `URL` params o cualquier fuente de usuario.

## Notas relacionadas

- [[02 JSON.stringify (replacer, space)|JSON.stringify]] — el proceso inverso; replacer complementa el reviver
- [[01 Sintaxis JSON|Sintaxis JSON]] — qué tipos pueden aparecer en el texto a parsear
- [[01 Fetch API/index|Fetch API]] — `res.json()` llama `JSON.parse` internamente sobre el body de la respuesta
