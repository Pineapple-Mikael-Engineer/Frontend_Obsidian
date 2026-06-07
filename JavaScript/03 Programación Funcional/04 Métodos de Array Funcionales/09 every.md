---
title: "every"
aliases:
  - Array every
  - array.every
tags:
  - javascript
  - api/metodo
  - arrays
draft: false
---

# every

> [!definicion]
> `Array.prototype.every` devuelve `true` si **todos** los elementos del array hacen que el callback retorne truthy. Hace cortocircuito — detiene la iteración en cuanto encuentra el primer elemento que falla. Si el array está vacío, devuelve `true` (vacuously true). Es el equivalente funcional del cuantificador universal: "¿todos los elementos...?".

```js
const edades = [22, 30, 45, 28];

const todosMayoresDeEdad = edades.every((edad) => edad >= 18);
// true (todos cumplen)

const todosMayoresDeTreinta = edades.every((edad) => edad > 30);
// false (22 y 28 no cumplen, para en 22)

// Array vacío → siempre true (vacuously true)
[].every((x) => x > 0);
// true
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
| Array vacío | Siempre `true` (vacuously true) |
| Cortocircuito | Sí — para al encontrar el primero falsy |
| Muta el array | No |
| Argumento `thisArg` | Opcional |
| Salta huecos (array sparse) | Sí |

## El array vacío y la verdad vacua

El resultado `true` para un array vacío puede ser contraintuitivo, pero es matemáticamente correcto: si no hay elementos que falsifiquen la condición (porque no hay ningún elemento), la condición se cumple para todos. Este comportamiento es consistente con la lógica de predicados.

```js
// La pregunta "¿todos los unicornios son azules?" es vacuamente verdadera
// porque no hay unicornios que la contradigan
[].every((unicornio) => unicornio.color === "azul");
// true

// Consecuencia práctica: siempre validar que el array no esté vacío
// si el resultado true en array vacío sería un falso positivo en tu dominio
const validarTodos = (arr, predicado) =>
  arr.length > 0 && arr.every(predicado);
```

## Ejemplos

### Validar que todos los campos de un formulario son válidos

```js
const campos = [
  { nombre: "email", valor: "ana@ejemplo.com", requerido: true },
  { nombre: "nombre", valor: "Ana García", requerido: true },
  { nombre: "teléfono", valor: "", requerido: false },
];

const formularioVálido = campos.every(
  (campo) => !campo.requerido || campo.valor.trim() !== ""
);
// true (el teléfono no es requerido, los requeridos tienen valor)
```

### Verificar que todos los items tienen una propiedad requerida

```js
const productos = [
  { nombre: "Laptop", precio: 1200, stock: 5 },
  { nombre: "Mouse", precio: 25, stock: 0 },
  { nombre: "Teclado", precio: 80 },
];

const todosConPrecio = productos.every((p) => p.precio !== undefined && p.precio > 0);
// true

const todosConStock = productos.every((p) => "stock" in p);
// false (Teclado no tiene stock)
```

### Comprobar que un array está ordenado

```js
const estáOrdenado = (arr) =>
  arr.every((n, i, a) => i === 0 || a[i - 1] <= n);

estáOrdenado([1, 2, 3, 4, 5]);   // true
estáOrdenado([1, 3, 2, 4, 5]);   // false
```

### Verificar que todos los elementos son del tipo esperado

```js
const esArrayDeStrings = (arr) =>
  Array.isArray(arr) && arr.every((x) => typeof x === "string");

esArrayDeStrings(["a", "b", "c"]);    // true
esArrayDeStrings(["a", 2, "c"]);      // false
esArrayDeStrings([]);                  // true (vacuously — decidir si es válido en tu dominio)
```

## Recetas comunes

### Validar que todos los permisos requeridos están presentes

```js
const verificarPermisos = (permisosUsuario, permisosRequeridos) =>
  permisosRequeridos.every((p) => permisosUsuario.includes(p));

verificarPermisos(["leer", "escribir", "admin"], ["leer", "escribir"]);  // true
verificarPermisos(["leer"], ["leer", "escribir"]);                        // false
```

### Confirmar que una operación batch fue exitosa

```js
const respuestas = [
  { ok: true, status: 200 },
  { ok: true, status: 201 },
  { ok: false, status: 404 },
];

const todasExitosas = respuestas.every((r) => r.ok);
// false → procesar errores

if (todasExitosas) {
  commitChanges();
} else {
  rollback();
}
```

### Verificar que una matriz es cuadrada

```js
const esMatrizCuadrada = (matriz) =>
  matriz.every((fila) => fila.length === matriz.length);

esMatrizCuadrada([[1, 2], [3, 4]]);       // true (2×2)
esMatrizCuadrada([[1, 2, 3], [4, 5]]);    // false
```

## Dualidad con `some`

`every` y `some` son duales mediante las leyes de De Morgan:

```js
// "todos cumplen fn" ≡ "no existe alguno que no cumpla fn"
arr.every(fn)  ≡  !arr.some(x => !fn(x))

// "alguno cumple fn" ≡ "no todos incumplen fn"
arr.some(fn)   ≡  !arr.every(x => !fn(x))
```

En términos prácticos: `every` es `some` negado con predicado negado.

## Cómo funciona por dentro

`every` itera desde el índice `0` hasta `length - 1`. En cada índice que sea propiedad propia del array, invoca el callback. En cuanto el callback devuelve un valor falsy, `every` devuelve `false` inmediatamente (cortocircuito). Si llega al final sin fallar ninguno, devuelve `true`. El array vacío nunca entra en el bucle, por eso devuelve `true`.

```js
// Implementación conceptual
Array.prototype.everyPropio = function(callback, thisArg) {
  for (let i = 0; i < this.length; i++) {
    if (i in this && !callback.call(thisArg, this[i], i, this)) {
      return false;  // cortocircuito
    }
  }
  return true;      // incluyendo array vacío
};
```

```
[2, 4, 6, 7].every(n => n % 2 === 0)

i=0: callback(2) → true → continúa
i=1: callback(4) → true → continúa
i=2: callback(6) → true → continúa
i=3: callback(7) → false → devuelve false, para
```

> [!tip] Buenas prácticas
> - Usar `every` para validaciones que requieren que todos los elementos cumplan una condición antes de proceder.
> - Nombrar la función predicada con claridad: `esVálido`, `cumpleRequisito`, `tieneStock`.
> - Cuando el `true` vacuo es problemático, añadir una comprobación de longitud: `arr.length > 0 && arr.every(predicado)`.
> - Para validaciones complejas de formularios, `every` sobre un array de objetos con `{ campo, valor, requerido }` es un patrón limpio y extensible.

> [!warning] Errores comunes
> - **Sorprenderse con el `true` vacuo:** `[].every(fn)` siempre es `true` — validar explícitamente la longitud si el array vacío no debe ser considerado válido.
> - **Usar `every` cuando basta con `some` negado:** a veces `!arr.some(fn)` es más legible que `arr.every(x => !fn(x))` — elegir la forma que exprese mejor la intención.
> - **Efectos secundarios en el callback:** el cortocircuito puede hacer que elementos al final del array nunca sean procesados — no mezclar efectos con `every`.
> - **Confundir cortocircuito con orden garantizado:** `every` se detiene al primero que falla, que puede no ser el "más importante" desde la perspectiva del dominio.
> - **`every` asíncrono:** como `some`, no es async-aware. Para predicados asíncronos, usar `Promise.all(arr.map(async fn)).then(results => results.every(Boolean))`.

## Notas relacionadas

- [[index|Métodos de Array Funcionales — Índice]]
- [[03 filter]]
- [[08 some]]
- [[06 find y findIndex]]
