---
title: Array — Acceso por Índice y length
aliases:
  - arr[i]
  - Array.at
  - array length
  - at()
tags:
  - javascript
  - api/metodo
  - arrays
objeto: Array
tipo: metodo
retorna: any
muta: false
asincrono: false
draft: false
order: 2
---

# Array — Acceso por Índice y `length`

> [!definicion]
> El acceso por corchete `arr[i]` lee la propiedad cuyo nombre es el string `String(i)` en el objeto array. Devuelve `undefined` si el índice no existe (fuera de rango o hueco). `arr.at(i)` (ES2022) permite índices negativos contando desde el final. La propiedad `length` vale el índice más alto registrado + 1, no necesariamente el número de elementos.

## Firmas

| Forma | Descripción | Muta |
|-------|-------------|------|
| `arr[i]` | Acceso por índice no negativo | no |
| `arr[i] = valor` | Asignación por índice | **sí** |
| `arr.at(i)` | Acceso con índice negativo permitido | no |
| `arr.length` | Longitud / índice máximo + 1 | no (lectura) |
| `arr.length = n` | Truncar o extender el array | **sí** |

## Acceso con `[]`

Los índices son enteros no negativos base 0. El acceso fuera de rango devuelve `undefined` sin lanzar error.

```js
const frutas = ["manzana", "pera", "uva"];

frutas[0];  // → "manzana"
frutas[2];  // → "uva"
frutas[5];  // → undefined  (fuera de rango, sin error)
frutas[-1]; // → undefined  ← -1 se convierte a "-1", no es índice válido
```

## Acceso con `at(i)` — índices negativos

`at(i)` admite índices negativos: `-1` es el último elemento, `-2` el penúltimo, etc. Equivale a `arr[arr.length + i]` cuando `i < 0`.

```js
const letras = ["a", "b", "c", "d"];

letras.at(0);   // → "a"
letras.at(-1);  // → "d"   (último)
letras.at(-2);  // → "c"   (penúltimo)
letras.at(10);  // → undefined
```

> [!tip] Último elemento
> Pre-ES2022: `arr[arr.length - 1]`. Post-ES2022: `arr.at(-1)`. Preferir `at(-1)` — más legible y sin riesgo de off-by-one en el cómputo de `length - 1`.

## Propiedad `length`

`length` es una propiedad mágica (con setter y getter propios) que el motor mantiene sincronizada con el índice numérico más alto del objeto.

```js
const a = [10, 20, 30];
a.length;  // → 3

// En un sparse array, length NO es el número de elementos
const sparse = [1, , , 4];
sparse.length; // → 4  (índice más alto es 3, length = 4)
Object.keys(sparse).length; // → 2  (solo hay dos propiedades reales: "0" y "3")
```

### Modificar `length` manualmente

Asignar un valor menor **trunca** el array (elimina elementos desde ese índice). Asignar un valor mayor crea huecos.

```js
const b = [1, 2, 3, 4, 5];

b.length = 3;
console.log(b); // → [1, 2, 3]  (4 y 5 eliminados)

b.length = 5;
console.log(b); // → [1, 2, 3, empty × 2]  (huecos creados)
console.log(3 in b); // → false
```

> [!warning] Errores comunes
> - `arr[-1]` devuelve `undefined`, no el último elemento. `-1` como clave de objeto es el string `"-1"`, que no es un índice de array válido.
> - Iterar con `for (let i = 0; i <= arr.length; i++)` — el `<=` accede un índice fuera de rango (`arr[arr.length]` → `undefined`). El límite correcto es `i < arr.length`.
> - Confiar en `length` para contar elementos en arrays sparse. Usar `Object.keys(arr).length` o `arr.filter(() => true).length` si se necesita el conteo real.
> - Modificar `length` directamente para vaciar un array es un antipatrón: `arr.length = 0` funciona pero es menos legible que `arr.splice(0)` o reasignar `arr = []`.

## Recetas comunes

```js
// Último elemento (dos estilos)
const ultimo = arr[arr.length - 1];  // pre-ES2022
const ultimo2 = arr.at(-1);          // ES2022+

// Verificar si el índice existe (distingue hueco de undefined real)
const i = 2;
if (i in arr) { /* el slot existe */ }

// Vaciar un array in-place (preservando la referencia)
arr.length = 0;

// Truncar a los primeros n elementos in-place
arr.length = n;
```

## Cómo funciona por dentro

`arr[0]` es syntactic sugar para `arr["0"]` — el motor convierte el número entero a string para buscar la propiedad en el objeto. Esta conversión ocurre a nivel de especificación (ToPropertyKey). `length` es una propiedad de datos con atributo `[[Writable]]: true` y getter/setter especiales definidos en `Array.prototype`: al asignar `arr[k] = v` con `k >= arr.length`, el motor ejecuta automáticamente `arr.length = k + 1`.

## Notas relacionadas

- [[01 Creación]]
- [[06 slice]]
- [[index|Arrays]]
