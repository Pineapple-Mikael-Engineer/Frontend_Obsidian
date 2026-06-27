---
title: Sintaxis JSON — tipos, reglas y diferencias con JS
aliases:
  - Sintaxis JSON
  - JSON tipos
tags:
  - javascript
  - api/concepto
  - datos
objeto: JSON
tipo: concepto
draft: false
order: 1
---

# Sintaxis JSON

> [!definicion]
> JSON es un subconjunto textual de la sintaxis de JavaScript, definido por la RFC 8259 con reglas más estrictas: solo 6 tipos de valor, claves de objeto obligatoriamente entre comillas dobles, sin trailing commas y sin comentarios. Un documento JSON válido es exactamente uno de los 6 tipos en su nivel raíz.

```json
{
  "id": 1,
  "nombre": "Ana",
  "activo": true,
  "saldo": 42.5,
  "etiquetas": ["admin", "beta"],
  "direccion": null
}
```

## Tipos permitidos en JSON

| Tipo JSON | Sintaxis | Ejemplo |
|---|---|---|
| string | Comillas dobles obligatorias | `"hola mundo"` |
| number | Entero o flotante, sin comillas | `42`, `-3.14`, `1e10` |
| boolean | Literal minúscula | `true`, `false` |
| null | Literal minúscula | `null` |
| array | Corchetes, elementos separados por coma | `[1, "dos", null]` |
| object | Llaves, claves-string: valor, coma | `{"a": 1, "b": 2}` |

## Tipos JS sin representación JSON

| Tipo / Valor JS | Resultado en JSON |
|---|---|
| `undefined` | Omitido (en objeto) o `null` (en array) |
| `function` | Omitida completamente |
| `Symbol` | Omitido |
| `NaN` | `null` |
| `Infinity` / `-Infinity` | `null` |
| `Date` | String ISO 8601 (`"2024-06-08T10:00:00.000Z"`) vía `toJSON()` |
| `Map` | `{}` (estructura vacía; contenido no serializable) |
| `Set` | `[]` (array vacío; contenido no serializable) |
| `BigInt` | Lanza `TypeError` |

La transformación de `Date` a string ISO es automática porque `Date.prototype.toJSON()` existe. Las demás clases (`Map`, `Set`) no tienen `toJSON()`, por lo que se serializan como su estructura JS desnuda (objeto vacío o array vacío).

## Reglas estrictas de JSON vs objeto JS literal

| Regla | JSON | Objeto JS |
|---|---|---|
| Claves | Siempre entre comillas dobles | Sin comillas permitido (`{a: 1}`) |
| Comillas en strings | Solo dobles (`"texto"`) | Simples o dobles |
| Trailing comma | Prohibida (`{"a":1,}` inválido) | Permitida |
| Comentarios | Prohibidos | Permitidos (`//`, `/* */`) |
| `undefined` como valor | Prohibido | Permitido |
| Valores numéricos especiales | `NaN`, `Infinity` prohibidos | Permitidos |

```js
// Objeto JS válido pero JSON inválido
const obj = {
  nombre: 'Ana',   // clave sin comillas
  fn: () => {},    // función → omitida en stringify
  fecha: new Date(), // Date → se convierte a string ISO
  // comentario → prohibido en JSON
};

JSON.stringify(obj);
// '{"nombre":"Ana","fecha":"2024-06-08T10:00:00.000Z"}'
// fn se omite, comentario no existe, nombre se entrecomilla
```

## Cómo funciona por dentro

JSON se valida mediante un analizador sintáctico (parser) que rechaza cualquier token fuera de la gramática. `JSON.parse()` lanza `SyntaxError` ante el primer token inválido: una coma extra, una clave sin comillas, o un `undefined` literal. Los parsers JSON son deliberadamente más estrictos que el motor JS porque operan sobre texto de red, donde el input no es de confianza.

> [!tip]
> Para validar JSON manualmente durante desarrollo: `JSON.parse(texto)` dentro de un `try/catch`. En VS Code, el Language Mode "JSON" marca errores de sintaxis en tiempo real.

> [!warning]
> `JSON.stringify(new Map([[1, 'a']]))` produce `'{}'`, no `'[[1,"a"]]'`. Si se necesita serializar un `Map`, convertirlo antes: `JSON.stringify([...miMapa])` o `JSON.stringify(Object.fromEntries(miMapa))`.

## Notas relacionadas

- [[02 JSON.stringify (replacer, space)|JSON.stringify]] — cómo se comporta con cada tipo durante la serialización
- [[03 JSON.parse (reviver)|JSON.parse]] — cómo se reconstruyen valores al deserializar
