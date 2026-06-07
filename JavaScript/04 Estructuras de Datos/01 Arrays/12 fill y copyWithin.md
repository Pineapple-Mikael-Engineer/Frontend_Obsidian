---
title: Array.prototype.fill y copyWithin — Rellenar y copiar segmentos in-place
aliases:
  - fill
  - copyWithin
  - Array.fill
  - Array.copyWithin
tags:
  - javascript
  - api/metodo
  - arrays
objeto: Array
tipo: metodo
retorna: Array
muta: true
asincrono: false
draft: false
---

# Array.prototype.fill y copyWithin

> [!definicion]
> `fill(valor, inicio?, fin?)` rellena el rango `[inicio, fin)` del array con el mismo `valor`. `copyWithin(destino, inicio?, fin?)` copia el segmento `[inicio, fin)` del propio array sobre la posición `destino`, sin cambiar la longitud. Ambos **mutan** el array y retornan el mismo array.

## Firmas

| Método | Firma | Retorna | Muta |
|--------|-------|---------|------|
| `fill` | `arr.fill(valor, inicio?, fin?)` | mismo array | **sí** |
| `copyWithin` | `arr.copyWithin(destino, inicio?, fin?)` | mismo array | **sí** |

## fill — rellenar con un valor

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `valor` | `any` | Valor con el que rellenar |
| `inicio` | `number` (opcional) | Índice de inicio, inclusivo. Por defecto `0`. Negativo: desde el final |
| `fin` | `number` (opcional) | Índice de fin, exclusivo. Por defecto `arr.length`. Negativo: desde el final |

```js
[1, 2, 3, 4, 5].fill(0);         // → [0, 0, 0, 0, 0]
[1, 2, 3, 4, 5].fill(9, 2);      // → [1, 2, 9, 9, 9]
[1, 2, 3, 4, 5].fill(9, 1, 3);   // → [1, 9, 9, 4, 5]
[1, 2, 3, 4, 5].fill(9, -2);     // → [1, 2, 3, 9, 9]
```

### Crear array de n ceros

```js
Array(5).fill(0);    // → [0, 0, 0, 0, 0]
Array(3).fill(null); // → [null, null, null]
Array(4).fill(1);    // → [1, 1, 1, 1]
```

`fill` inicializa los huecos de un sparse array como propiedades reales, a diferencia de simplemente asignar `length`.

### Trampa: `fill` con objetos comparte referencia

```js
// MAL — todas las posiciones son el MISMO objeto
const mal = Array(3).fill({});
mal[0].x = 1;
console.log(mal[1].x); // → 1  (compartido)
console.log(mal[2].x); // → 1  (compartido)

// BIEN — un objeto distinto por posición
const bien = Array.from({ length: 3 }, () => ({}));
bien[0].x = 1;
console.log(bien[1].x); // → undefined  (independiente)
```

El mismo problema aplica a arrays: `Array(3).fill([])` crea tres referencias al mismo array interno.

## copyWithin — copiar segmento sobre sí mismo

`copyWithin` copia una porción del propio array sobre otra posición, sin crear elementos nuevos ni cambiar `length`. La copia ocurre antes de sobrescribir (no hay colisiones).

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `destino` | `number` | Índice donde empieza a pegar. Negativo: desde el final |
| `inicio` | `number` (opcional) | Inicio del segmento a copiar. Por defecto `0` |
| `fin` | `number` (opcional) | Fin del segmento (exclusivo). Por defecto `arr.length` |

```js
[1, 2, 3, 4, 5].copyWithin(0, 3);
// Copia [4, 5] (índices 3-4) sobre la posición 0
// → [4, 5, 3, 4, 5]

[1, 2, 3, 4, 5].copyWithin(1, 3, 5);
// Copia [4, 5] sobre posición 1
// → [1, 4, 5, 4, 5]

[1, 2, 3, 4, 5].copyWithin(-2);
// Destino = índice 3; fuente = todo el array (índices 0-4)
// → [1, 2, 3, 1, 2]
```

### Cuándo usar `copyWithin`

`copyWithin` es raro en código de aplicación. Sus casos de uso legítimos:

- Algoritmos de bajo nivel sobre buffers que evitan asignaciones.
- **Typed Arrays** (`Int32Array`, `Float64Array`) donde la ausencia de GC y la manipulación en memoria es crítica.
- Implementaciones de algoritmos in-place que necesitan desplazar elementos sin crear copias intermedias.

```js
// Desplazar elementos a la derecha sin crear array nuevo
const buf = new Int32Array([1, 2, 3, 4, 5]);
buf.copyWithin(1, 0, 4); // → [1, 1, 2, 3, 4]  — desplazamiento a la derecha
```

> [!tip] Buenas prácticas
> - Usar `fill` para inicializar arrays de longitud fija con valores primitivos.
> - Para objetos únicos por posición, siempre `Array.from({ length: n }, () => ({ ... }))`.
> - Reservar `copyWithin` para optimizaciones de bajo nivel o Typed Arrays; en código de aplicación, `slice` + spread son más legibles.

> [!warning] Errores comunes
> - `Array(n).fill({})` o `Array(n).fill([])` — referencia compartida. El objeto/array se comparte entre todos los índices.
> - `fill` retorna el mismo array mutado; encadenarlo después de `Array(n)` es correcto (`Array(5).fill(0)`) porque retorna el array que luego se puede usar.
> - `copyWithin` no cambia `length` — no se puede usar para "extender" un array.

## Notas relacionadas

- [[01 Creación]]
- [[06 slice]]
- [[14 Arrays Multidimensionales]]
- [[index|Arrays]]
