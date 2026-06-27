---
title: "some"
aliases:
  - Array some
  - array.some
tags:
  - javascript
  - api/metodo
  - arrays
draft: false
order: 8
---

# some

> [!definicion]
> `Array.prototype.some` devuelve `true` si **al menos un elemento** del array hace que el callback retorne truthy. Hace cortocircuito — detiene la iteración en cuanto encuentra la primera coincidencia. Si el array está vacío, siempre devuelve `false`. Es el equivalente funcional del cuantificador existencial: "¿existe algún elemento que...?".

```js
const edades = [15, 22, 17, 30, 14];

const hayMayorDeEdad = edades.some((edad) => edad >= 18);
// true (22 y 30 cumplen, para en 22)

const hayMayorDeCien = edades.some((edad) => edad > 100);
// false (ninguno cumple, recorre todos)

// Array vacío → siempre false
[].some((x) => x > 0);
// false
```

## Firma del método

| Parámetro del callback | Tipo | Descripción |
|---|---|---|
| `elemento` | any | Valor del elemento actual |
| `índice` | number | Posición (0-based) |
| `array` | Array | Referencia al array original |

| Aspecto | Detalle |
|---|---|
| Valor de retorno | `boolean` |
| Array vacío | Siempre `false` |
| Cortocircuito | Sí — para al encontrar el primero truthy |
| Muta el array | No |
| Argumento `thisArg` | Opcional |
| Salta huecos (array sparse) | Sí |

## Ejemplos

### Comprobar si hay errores en una lista de respuestas

```js
const respuestas = [
  { url: "/api/users", status: 200 },
  { url: "/api/orders", status: 500 },
  { url: "/api/products", status: 200 },
];

const hayError = respuestas.some((r) => r.status >= 400);
// true → activar banner de error
```

### Verificar permisos de un usuario

```js
const permisosUsuario = ["leer", "comentar", "votar"];
const permisosRequeridos = ["administrar", "editar"];

const tieneAlgúnPermiso = permisosRequeridos.some((p) =>
  permisosUsuario.includes(p)
);
// false (no tiene "administrar" ni "editar")
```

### Comprobar si algún campo de un formulario está vacío

```js
const campos = ["nombre", "email", "teléfono"];
const datos = { nombre: "Ana", email: "", teléfono: "666123456" };

const hayVacíos = campos.some((campo) => !datos[campo]);
// true ("email" está vacío)
```

### Validar que un array no tiene duplicados

```js
const hayDuplicados = (arr) =>
  arr.some((elem, i, a) => a.indexOf(elem) !== i);

hayDuplicados([1, 2, 3, 4]);      // false
hayDuplicados([1, 2, 2, 4]);      // true
```

### Comprobar existencia antes de operar

```js
const usuarios = [{ id: 1 }, { id: 2 }, { id: 3 }];

if (usuarios.some((u) => u.id === 2)) {
  console.log("El usuario 2 existe");
}
// Más expresivo que: usuarios.find(u => u.id === 2) !== undefined
// Más eficiente que: usuarios.filter(u => u.id === 2).length > 0
```

## Recetas comunes

### Implementar "alguno de estos valores existe en el array"

```js
const incluirAlgunos = (arr, valores) =>
  valores.some((v) => arr.includes(v));

incluirAlgunos(["a", "b", "c"], ["x", "b", "y"]);  // true ("b" está)
incluirAlgunos(["a", "b", "c"], ["x", "y", "z"]);  // false
```

### Validación de formularios con mensajes de error específicos

```js
const reglas = [
  { test: (v) => !v, mensaje: "El campo es obligatorio" },
  { test: (v) => v.length < 3, mensaje: "Mínimo 3 caracteres" },
  { test: (v) => !/^[a-z]+$/i.test(v), mensaje: "Solo letras" },
];

const validar = (valor) => {
  const error = reglas.find((r) => r.test(valor));
  return error ? error.mensaje : null;
};

validar("");        // "El campo es obligatorio"
validar("ab");     // "Mínimo 3 caracteres"
validar("ab3");    // "Solo letras"
validar("abc");    // null (válido)
```

### Detección de características del entorno

```js
const formatos = ["webp", "avif", "jpeg"];
const formatoSoportado = formatos.some(
  (fmt) => document.createElement("canvas").toDataURL(`image/${fmt}`).startsWith(`data:image/${fmt}`)
);
```

## `some` vs `find` vs `filter`

```js
const numeros = [1, 3, 5, 4, 2];

// some → ¿existe alguno que cumpla? → boolean
numeros.some((n) => n % 2 === 0);      // true

// find → ¿cuál es el primero que cumple? → elemento
numeros.find((n) => n % 2 === 0);      // 4

// filter → ¿cuáles cumplen? → array de elementos
numeros.filter((n) => n % 2 === 0);    // [4, 2]
```

## Dualidad con `every`

`some` y `every` son duales lógicos mediante las leyes de De Morgan:

```js
// "alguno cumple fn" ≡ "no todos incumplen fn"
arr.some(fn)  ≡  !arr.every(x => !fn(x))

// "todos cumplen fn" ≡ "ninguno incumple fn"
arr.every(fn) ≡  !arr.some(x => !fn(x))
```

En la práctica, elegir el que hace el código más legible según la pregunta que se hace.

## Cómo funciona por dentro

`some` itera desde el índice `0` hasta `length - 1`. En cada índice que sea propiedad propia del array, invoca el callback. En cuanto el callback devuelve un valor truthy, `some` devuelve `true` inmediatamente (cortocircuito). Si termina la iteración sin encontrar ninguno truthy, devuelve `false`. El array vacío hace que el bucle no se ejecute nunca, por eso devuelve `false`.

```js
// Implementación conceptual
Array.prototype.somePropio = function(callback, thisArg) {
  for (let i = 0; i < this.length; i++) {
    if (i in this && callback.call(thisArg, this[i], i, this)) {
      return true;   // cortocircuito
    }
  }
  return false;
};
```

> [!tip] Buenas prácticas
> - Preferir `some` sobre `filter(...).length > 0` o `find(...) !== undefined` cuando solo necesitas una respuesta booleana — es más semántico y eficiente.
> - Usar `some` para guardianes (early exit): `if (!datos.some(validar)) return;`.
> - El nombre del callback como predicado mejora la legibilidad: `estaActivo`, `tienePermiso`, `esMayorDeEdad`.
> - `some` con predicados puros es fácil de testear unitariamente.

> [!warning] Errores comunes
> - **Confundir el resultado de array vacío:** `[].some(fn)` es siempre `false` — no lanza error. Esto puede dar falsos negativos si se pasa accidentalmente un array vacío.
> - **Usar `some` en lugar de `find` cuando necesitas el elemento:** `some` solo devuelve booleano. Si necesitas el valor, usar `find`.
> - **Efectos secundarios en el callback:** si el callback tiene efectos, el cortocircuito puede hacer que algunos no se ejecuten — comportamiento no determinista según el contenido del array.
> - **No cortocircuito garantizado en promesas:** `some` no es async-aware. Para comprobar con predicados asíncronos, usar `Promise.all` con `map` o un bucle `for...of`.

## Notas relacionadas

- [[index|Métodos de Array Funcionales — Índice]]
- [[03 filter]]
- [[06 find y findIndex]]
- [[09 every]]
