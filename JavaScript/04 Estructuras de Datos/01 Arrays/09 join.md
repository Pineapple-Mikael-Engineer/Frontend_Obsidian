---
title: Array.prototype.join — Convertir a string con separador
aliases:
  - join
  - Array.join
tags:
  - javascript
  - api/metodo
  - arrays
objeto: Array
tipo: metodo
retorna: string
muta: false
asincrono: false
draft: false
order: 9
---

# Array.prototype.join

> [!definicion]
> `arr.join(separador?)` convierte cada elemento del array a string y los une concatenando el separador entre cada par. Retorna el string resultante. `null` y `undefined` se convierten a string vacío `""`. No muta el array. Por defecto el separador es `","`.

## Firma

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `separador` | `string` (opcional) | String que se inserta entre cada elemento. Por defecto `","` |

**Retorna:** `string`. **No muta.**

## Ejemplos base

```js
["a", "b", "c"].join();        // → "a,b,c"   (separador por defecto)
["a", "b", "c"].join(", ");    // → "a, b, c"
["a", "b", "c"].join("");      // → "abc"      (sin separador)
["a", "b", "c"].join(" - ");   // → "a - b - c"
[1, 2, 3].join("+");           // → "1+2+3"
```

## Comportamiento con `null`, `undefined` y huecos

`null`, `undefined` y los huecos de arrays sparse se convierten a string vacío `""`:

```js
[1, null, 3].join("-");        // → "1--3"
[1, undefined, 3].join("-");   // → "1--3"
[1, , 3].join("-");            // → "1--3"

// Los objetos llaman a su .toString()
[{ toString: () => "X" }, 2].join("-"); // → "X-2"
```

## Recetas comunes

### Construir rutas o URLs

```js
const segmentos = ["api", "v2", "usuarios", "42"];
segmentos.join("/"); // → "api/v2/usuarios/42"
```

### Generar CSV

```js
const fila = ["Juan", 30, "Madrid"];
fila.join(","); // → "Juan,30,Madrid"

const filas = [["Nombre", "Edad"], ["Ana", 25], ["Luis", 28]];
filas.map(f => f.join(",")).join("\n");
// → "Nombre,Edad\nAna,25\nLuis,28"
```

### Construir clases CSS dinámicas

```js
const clases = ["btn", conError && "btn-error", activo && "btn-activo"].filter(Boolean);
clases.join(" "); // → "btn btn-error"  (si conError es true y activo es false)
```

### Construir strings repetidos (sin `repeat`)

```js
Array(5).fill("*").join(""); // → "*****"
Array(3).fill("--").join("+"); // → "--+--+--"
```

## `join` vs `toString`

`arr.toString()` equivale a `arr.join(",")` — el motor llama `join` internamente. No hay diferencia funcional, pero `join` es más explícito y permite separadores personalizados.

```js
[1, 2, 3].toString(); // → "1,2,3"
[1, 2, 3].join(",");  // → "1,2,3"  (idéntico)
```

## El patrón split → transformar → join

Un idioma común para transformar strings:

```js
// Convertir snake_case a camelCase
"hola_mundo_js".split("_").map((p, i) =>
  i === 0 ? p : p[0].toUpperCase() + p.slice(1)
).join(""); // → "holaMundoJs"

// Invertir palabras de una frase
"hola mundo".split(" ").reverse().join(" "); // → "mundo hola"
```

> [!tip] Buenas prácticas
> - Usar `join("")` para concatenar muchos strings de un array — más eficiente que reducir con `+` (el motor puede optimizar la asignación del buffer).
> - Para construir HTML desde templates, `join` es válido pero en aplicaciones modernas conviene `template literals` o JSX/DOM para evitar vulnerabilidades XSS con contenido de usuario.

> [!warning] Errores comunes
> - Olvidar que `null`/`undefined` → `""`: `[1, null, 2].join("-")` produce `"1--2"`, no `"1-null-2"`.
> - Llamar `join` sobre arrays de objetos sin `toString` personalizado: los objetos se convierten a `"[object Object]"`.
> - Confundir `join` (array → string) con `split` (string → array): son operaciones inversas.

## Notas relacionadas

- [[08 includes, indexOf, lastIndexOf]]
- [[07 concat]]
- [[index|Arrays]]
