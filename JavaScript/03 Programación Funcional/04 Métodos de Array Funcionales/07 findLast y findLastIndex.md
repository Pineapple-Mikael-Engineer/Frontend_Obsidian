---
title: "findLast y findLastIndex"
aliases:
  - Array findLast
  - Array findLastIndex
  - array.findLast
  - array.findLastIndex
tags:
  - javascript
  - api/metodo
  - arrays
draft: false
---

# findLast y findLastIndex

> [!definicion]
> `Array.prototype.findLast` devuelve el **último elemento** del array para el que el callback retorna truthy, iterando de derecha a izquierda. `Array.prototype.findLastIndex` devuelve el **índice** de ese último elemento. Son los equivalentes inversos de `find` y `findIndex`, introducidos en **ES2023**. Si no encuentran nada, `findLast` devuelve `undefined` y `findLastIndex` devuelve `-1`.

```js
const registros = [
  { id: 1, tipo: "info", mensaje: "Inicio" },
  { id: 2, tipo: "error", mensaje: "Fallo de red" },
  { id: 3, tipo: "info", mensaje: "Reintentando" },
  { id: 4, tipo: "error", mensaje: "Timeout" },
  { id: 5, tipo: "info", mensaje: "Recuperado" },
];

const últimoError = registros.findLast((r) => r.tipo === "error");
// {id: 4, tipo: "error", mensaje: "Timeout"}

const índiceÚltimoError = registros.findLastIndex((r) => r.tipo === "error");
// 3
```

## Firma del método

| Parámetro del callback | Tipo | Descripción |
|---|---|---|
| `elemento` | any | Valor del elemento actual (visitado de derecha a izquierda) |
| `índice` | number | Posición real en el array (preserva los índices originales) |
| `array` | Array | Referencia al array original |

| Aspecto | `findLast` | `findLastIndex` |
|---|---|---|
| Devuelve si encuentra | El elemento | El índice (`number`) |
| Devuelve si no encuentra | `undefined` | `-1` |
| Orden de iteración | Derecha a izquierda | Derecha a izquierda |
| Cortocircuito | Sí (al encontrar el primero desde la derecha) | Sí |
| Disponibilidad | ES2023 | ES2023 |
| Muta el array | No | No |

## Compatibilidad (ES2023)

`findLast` y `findLastIndex` están disponibles desde:

- Chrome 97+ / Edge 97+ (enero 2022)
- Firefox 104+ (agosto 2022)
- Safari 15.4+ (marzo 2022)
- Node.js 18+ (sin flag)

Para entornos que no soporten ES2023, usar un polyfill o la alternativa pre-ES2023.

## Ejemplos

### Buscar la última transacción de un tipo

```js
const transacciones = [
  { id: 1, tipo: "compra", monto: 100 },
  { id: 2, tipo: "venta", monto: 200 },
  { id: 3, tipo: "compra", monto: 150 },
  { id: 4, tipo: "compra", monto: 300 },
];

const últimaCompra = transacciones.findLast((t) => t.tipo === "compra");
// {id: 4, tipo: "compra", monto: 300}
```

### Encontrar el último elemento que cumple una condición numérica

```js
const precios = [120, 80, 200, 45, 310, 95, 180];

const últimoMenorDeCien = precios.findLast((p) => p < 100);
// 95

const índice = precios.findLastIndex((p) => p < 100);
// 5
```

### Búsqueda en historial de estados (patrón común en gestión de estado)

```js
const historial = [
  { accion: "AGREGAR", id: 1 },
  { accion: "EDITAR", id: 1 },
  { accion: "AGREGAR", id: 2 },
  { accion: "ELIMINAR", id: 1 },
  { accion: "AGREGAR", id: 3 },
];

const últimaAcciónSobreId2 = historial.findLast((h) => h.id === 2);
// {accion: "AGREGAR", id: 2}  (solo hay una, pero en patrones complejos puede haber varias)

const últimoAgregar = historial.findLast((h) => h.accion === "AGREGAR");
// {accion: "AGREGAR", id: 3}
```

## Alternativa pre-ES2023

Antes de ES2023, el patrón habitual era combinar `reverse()` con `find()`, pero esto tiene una trampa:

```js
// MAL: reverse() muta el array original
const arr = [1, 2, 3, 4, 5];
const último = arr.reverse().find((n) => n < 4);
// 3 (correcto), pero arr ahora es [5, 4, 3, 2, 1] — array mutado

// BIEN: copiar primero con spread o slice
const último2 = [...arr].reverse().find((n) => n < 4);
// Correcto y arr no se muta, pero crea una copia extra del array
```

`findLast` evita crear la copia y es más eficiente para arrays grandes:

```js
// ES2023 — sin copia, sin mutación, cortocircuito nativo
arr.findLast((n) => n < 4);
```

Otra alternativa pre-ES2023 sin copiar (menos idiomática):

```js
function findLast(arr, predicado) {
  for (let i = arr.length - 1; i >= 0; i--) {
    if (predicado(arr[i], i, arr)) return arr[i];
  }
  return undefined;
}
```

## Cómo funciona por dentro

Ambos métodos inician la iteración en `length - 1` y avanzan hacia `0`. Los índices preservan sus valores originales — el callback recibe el índice real del elemento en el array, no una posición invertida. Cuando el callback devuelve truthy, `findLast` devuelve `array[i]` y `findLastIndex` devuelve `i`, y la iteración se detiene. Los huecos en arrays sparse se saltan.

```
[10, 20, 30, 40, 50].findLast((n) => n < 35)

i=4: callback(50, 4) → false
i=3: callback(40, 3) → false
i=2: callback(30, 2) → true → devuelve 30, para
```

> [!tip] Buenas prácticas
> - Usar `findLast`/`findLastIndex` cuando la semántica es "la última ocurrencia que cumple X" — es más expresivo que la alternativa con `reverse`.
> - Verificar compatibilidad del entorno o añadir un polyfill para proyectos que soporten navegadores antiguos.
> - No usar `[...arr].reverse().find(fn)` si está disponible `findLast` — la copia adicional es innecesaria.
> - Cuando necesitas tanto el elemento como su índice, `findLastIndex` es la base: `const i = arr.findLastIndex(fn); const elem = arr[i];`.

> [!warning] Errores comunes
> - **Asumir disponibilidad sin verificar:** ES2023 no está soportado en Node.js < 18 ni en navegadores muy antiguos. Comprobar el target del proyecto.
> - **Usar `arr.reverse().find()` sin copiar:** muta el array original. Siempre crear una copia si no se puede usar `findLast`.
> - **Confundir el índice devuelto:** `findLastIndex` devuelve el índice real (posición original en el array), no la posición desde la derecha.
> - **Esperar que devuelva `null`:** devuelve `undefined` cuando no encuentra, igual que `find`.

## Notas relacionadas

- [[index|Métodos de Array Funcionales — Índice]]
- [[06 find y findIndex]]
- [[08 some]]
