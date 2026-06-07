---
title: string — Cadenas de texto en JavaScript (UTF-16 inmutables)
aliases:
  - string JS
  - tipo string
  - cadena de texto
tags:
  - javascript
  - api/concepto
  - tipos
objeto: String
tipo: concepto
retorna: string
muta: false
draft: false
---

# string

> [!definicion]
> Un `string` en JavaScript es una **secuencia inmutable de unidades de código UTF-16**. Toda operación que "modifica" un string produce un string nuevo; el original permanece intacto. Es un tipo primitivo; el motor aplica auto-boxing para acceder a los métodos del prototipo `String`.

```js
typeof "hola"          // → "string"
typeof 'mundo'         // → "string"
typeof `template`      // → "string"

let s = "texto";
s.toUpperCase();       // → "TEXTO"
console.log(s);        // → "texto"  (inmutable)
```

## Tres formas de literales

```js
'comillas simples'          // sin interpolación
"comillas dobles"           // sin interpolación
`template literal`          // con interpolación y multilínea
```

Las comillas simples y dobles son equivalentes. La convención habitual es elegir una y ser consistente; usar la contraria para evitar escape (`"it's"` o `'say "hi"'`).

## Template literals

Los template literals (backticks) permiten:

**Interpolación** con `${}`: cualquier expresión JS dentro de las llaves se evalúa y se convierte a string.

```js
const nombre = "Ana";
const edad = 30;
`Hola, ${nombre}. Tienes ${edad} años.`
// → "Hola, Ana. Tienes 30 años."

`Resultado: ${2 ** 10}`   // → "Resultado: 1024"
`${true ? "si" : "no"}`   // → "si"
```

**Multilínea** sin `\n` explícito:

```js
const html = `
  <div>
    <p>Hola</p>
  </div>
`;
```

**Tagged templates**: una función prefija el backtick y procesa las partes del string.

```js
function tag(strings, ...valores) {
  // strings: array de partes literales
  // valores: array de expresiones interpoladas
  return strings.raw[0] + valores[0].toUpperCase();
}

tag`Hola, ${"ana"}!`
// strings.raw[0] → "Hola, "
// valores[0]     → "ana"
// → "Hola, ANA"
```

Los tagged templates se usan en bibliotecas como `gql` (GraphQL), `css` (CSS-in-JS), `html` (sanitización), y `sql` para queries parametrizadas.

## Métodos importantes

| Método | Retorna | Muta | Descripción |
|---|---|---|---|
| `.length` | `number` | no | Numero de unidades UTF-16 |
| `.charAt(i)` | `string` | no | Caracter en posicion `i` |
| `[i]` | `string` | no | Acceso por indice (equivale a `charAt`) |
| `.at(i)` | `string` | no | Acepta indices negativos (`at(-1)` = ultimo) |
| `.indexOf(sub)` | `number` | no | Primera posicion de `sub` o `-1` |
| `.lastIndexOf(sub)` | `number` | no | Ultima posicion de `sub` o `-1` |
| `.includes(sub)` | `boolean` | no | `true` si contiene `sub` |
| `.startsWith(sub)` | `boolean` | no | `true` si empieza con `sub` |
| `.endsWith(sub)` | `boolean` | no | `true` si termina con `sub` |
| `.slice(i, j)` | `string` | no | Extrae desde `i` hasta `j` (exclusivo); negativos ok |
| `.substring(i, j)` | `string` | no | Como `slice` pero sin negativos |
| `.toUpperCase()` | `string` | no | Convierte a mayusculas |
| `.toLowerCase()` | `string` | no | Convierte a minusculas |
| `.trim()` | `string` | no | Elimina espacios en los extremos |
| `.trimStart()` | `string` | no | Elimina espacios al inicio |
| `.trimEnd()` | `string` | no | Elimina espacios al final |
| `.padStart(n, fill)` | `string` | no | Rellena hasta longitud `n` por la izquierda |
| `.padEnd(n, fill)` | `string` | no | Rellena hasta longitud `n` por la derecha |
| `.repeat(n)` | `string` | no | Repite el string `n` veces |
| `.replace(pat, rep)` | `string` | no | Reemplaza la primera coincidencia |
| `.replaceAll(pat, rep)` | `string` | no | Reemplaza todas las coincidencias |
| `.split(sep)` | `Array` | no | Divide en array por separador |

```js
"  hola mundo  ".trim()        // → "hola mundo"
"abc".padStart(5, "0")         // → "00abc"
"ha".repeat(3)                 // → "hahaha"
"a,b,c".split(",")             // → ["a", "b", "c"]
"hola mundo".slice(5)          // → "mundo"
"hola mundo".slice(0, 4)       // → "hola"
"hola mundo".at(-5)            // → "m"
"color: red".replace("red", "blue")  // → "color: blue"
```

## Recetas comunes

```js
// Verificar si un string contiene una subcadena
"javascript".includes("script")   // → true

// Capitalizar primera letra
s => s.charAt(0).toUpperCase() + s.slice(1)

// Rellenar un número con ceros a la izquierda
String(7).padStart(3, "0")        // → "007"

// Dividir un CSV en tokens
"a,b,,c".split(",")               // → ["a", "b", "", "c"]

// Eliminar duplicados de espacios
"  hola   mundo  ".trim().replace(/\s+/g, " ")   // → "hola mundo"
```

## Strings y Unicode

JavaScript usa UTF-16 internamente. Los caracteres del plano básico (BMP, U+0000 a U+FFFF) ocupan una unidad de código (una posición de `length`). Los caracteres fuera del BMP (emoji, algunos símbolos, caracteres CJK raros) se representan como **pares sustitutos**: dos unidades UTF-16 = dos posiciones de `length`.

```js
"😀".length         // → 2  (par sustituto: 😀)
"A".length          // → 1
[..."😀"].length    // → 1  (spread usa iterador, que maneja code points)
```

Para trabajar con code points (no unidades):

```js
"😀".codePointAt(0)        // → 128512  (U+1F600)
String.fromCodePoint(128512) // → "😀"
```

## Auto-boxing

Cuando se accede a un método sobre un string primitivo, el motor crea temporalmente un `new String(valor)`, ejecuta el método, y descarta el wrapper. El primitivo en sí no cambia.

```js
// Lo que parece:
"hola".toUpperCase()

// Lo que ejecuta el motor:
new String("hola").toUpperCase()   // → "HOLA"
// El objeto String se descarta; "hola" permanece
```

> [!tip] Buenas prácticas
> - Preferir template literals para concatenar variables: más legible que `+`.
> - Usar `.at(-1)` para acceder al último elemento en lugar de `s[s.length - 1]`.
> - Para comparar strings en lógica de búsqueda, normalizar primero: `.toLowerCase()` antes de `.includes()`.
> - Para strings con caracteres Unicode fuera del BMP, iterar con `for...of` o spread `[...s]` en lugar de `for` con índice.

> [!warning] Errores comunes
> - `"😀".length === 2`, no 1: el índice numérico no equivale a "carácter".
> - `.replace(patron, reemplazo)` solo reemplaza la **primera** coincidencia si `patron` es string; usar `.replaceAll()` o una regex con flag `g` para todas.
> - `"3" + 1` → `"31"` (concatenación), no `4`. El operador `+` con strings hace concatenación.
> - `substring(i, j)` intercambia `i` y `j` si `i > j`; `.slice(i, j)` no lo hace (retorna `""`).

## Notas relacionadas

- [[04 Conversión de Tipos/01 Conversión Explícita (Number, String, Boolean)|Conversión Explícita]] — `String(valor)` vs `.toString()`
- [[04 Conversión de Tipos/02 Conversión Implícita|Conversión Implícita]] — el operador `+` y la concatenación
- [[01 Primitivos/index|Primitivos]]
