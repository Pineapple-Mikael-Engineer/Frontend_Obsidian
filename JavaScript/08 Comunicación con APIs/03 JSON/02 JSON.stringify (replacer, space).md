---
title: JSON.stringify — serializar valores JS a texto JSON
aliases:
  - JSON.stringify
  - stringify
tags:
  - javascript
  - api/metodo
  - datos
objeto: JSON
tipo: metodo
retorna: string | undefined
muta: false
asincrono: false
draft: false
---

# `JSON.stringify` — serializar a texto JSON

> [!definicion]
> `JSON.stringify(valor, replacer?, space?)` convierte un valor JS a su representación como string JSON. Retorna `undefined` si el valor raíz no es serializable (funciones, `Symbol`, `undefined`). Los valores no serializables dentro de objetos se omiten; dentro de arrays se reemplazan por `null`.

```js
JSON.stringify({ a: 1, b: 'hola', c: true });
// '{"a":1,"b":"hola","c":true}'

JSON.stringify([1, undefined, () => {}, 'texto']);
// '[1,null,null,"texto"]'    ← undefined y función → null en array
```

## Firma completa

```js
JSON.stringify(valor)
JSON.stringify(valor, replacer)
JSON.stringify(valor, replacer, space)
```

## Parámetro `replacer`

Controla qué propiedades se incluyen en la salida. Acepta dos formas:

### Array de strings (whitelist de keys)

Solo incluye las propiedades cuyos nombres estén en el array. El orden del output sigue el array.

```js
const usuario = { id: 1, nombre: 'Ana', password: 'secreto', rol: 'admin' };
JSON.stringify(usuario, ['id', 'nombre', 'rol']);
// '{"id":1,"nombre":"Ana","rol":"admin"}'    ← password excluido
```

### Función `(key, value) => value | undefined`

Se llama recursivamente para cada clave. Retornar `undefined` omite la propiedad; retornar el value modificado lo transforma.

```js
JSON.stringify({ a: 1, b: 2, c: 3 }, (key, value) => {
  if (key === '') return value;  // llamada raíz: siempre pasar
  return value > 1 ? value : undefined;
});
// '{"b":2,"c":3}'
```

La primera llamada tiene `key === ''` y `value` es el objeto raíz: debe retornarse siempre o se omite todo.

### Receta: omitir campos privados (clave que empieza con `_`)

```js
const estado = {
  id: 42,
  nombre: 'Sesión activa',
  _token: 'abc123',
  _timestamp: Date.now()
};

JSON.stringify(estado, (key, value) => {
  if (key === '') return value;
  return key.startsWith('_') ? undefined : value;
});
// '{"id":42,"nombre":"Sesión activa"}'
```

## Parámetro `space`

Controla la indentación del output. Sin `space`, el JSON es compacto (sin espacios ni saltos).

| Valor de `space` | Efecto |
|---|---|
| `0` o `null` | Sin formato (compacto) |
| Número 1–10 | N espacios de indentación |
| String (máx 10 chars) | El string como unidad de indentación (p. ej. `'\t'`) |

```js
// Pretty-print con 2 espacios (lo más habitual para logs y archivos)
console.log(JSON.stringify({ a: 1, b: [2, 3] }, null, 2));
// {
//   "a": 1,
//   "b": [
//     2,
//     3
//   ]
// }

// Indentación con tabuladores
JSON.stringify(obj, null, '\t');
```

## Comportamiento con valores especiales

| Valor JS | Resultado en `JSON.stringify` |
|---|---|
| `undefined` en objeto | Propiedad omitida |
| `undefined` en array | `null` |
| `function` en objeto | Propiedad omitida |
| `function` en array | `null` |
| `NaN` | `"null"` (string `null`) |
| `Infinity` / `-Infinity` | `"null"` |
| `Date` | String ISO 8601 (vía `toJSON()`) |
| `Map` | `{}` |
| `Set` | `[]` o `{}` según implementación (contenido perdido) |
| `BigInt` | Lanza `TypeError` |
| `Symbol` | Omitido (igual que `undefined`) |

## `toJSON()`

Si el valor tiene un método `toJSON()`, `JSON.stringify` lo llama y serializa su resultado en lugar del objeto original. `Date` lo usa internamente para producir el string ISO.

```js
class Temperatura {
  constructor(celsius) { this.celsius = celsius; }
  toJSON() {
    return { C: this.celsius, F: this.celsius * 9/5 + 32 };
  }
}

JSON.stringify(new Temperatura(100));
// '{"C":100,"F":212}'
```

## Cómo funciona por dentro

`JSON.stringify` recorre el objeto en **profundidad primero** (DFS). Para cada valor: si tiene `toJSON`, lo llama; si hay `replacer` función, lo aplica; luego convierte según el tipo. Para objetos, itera `Object.keys()` (propiedades enumerables propias). Las referencias circulares lanzan `TypeError: Converting circular structure to JSON`.

```js
const a = {};
a.self = a;
JSON.stringify(a); // TypeError: Converting circular structure to JSON
```

> [!tip]
> `JSON.stringify(obj, null, 2)` es el estándar para pretty-print en logs de desarrollo, archivos de configuración y payloads que se van a inspeccionar. Para producción, siempre sin `space` para minimizar el tamaño del payload.

> [!warning]
> `JSON.stringify` sobre un objeto con propiedades `Symbol` como clave (no como valor) las ignora silenciosamente. También ignora propiedades no enumerables (`Object.defineProperty` con `enumerable: false`). Esto significa que `JSON.stringify` no es un deep clone: recuperar el objeto con `JSON.parse` puede producir un objeto diferente al original.

## Notas relacionadas

- [[03 JSON.parse (reviver)|JSON.parse]] — el proceso inverso; reviver complementa el replacer
- [[01 Sintaxis JSON|Sintaxis JSON]] — por qué ciertos tipos no tienen representación JSON
- [[01 open y send|XMLHttpRequest — open y send]] — contexto de uso: serializar el body antes de enviarlo
