---
title: "Encadenamiento de Métodos"
aliases:
  - Method chaining arrays
  - Pipeline de arrays
  - Encadenar métodos funcionales
tags:
  - javascript
  - api/metodo
  - arrays
draft: false
order: 12
---

# Encadenamiento de Métodos

> [!definicion]
> El encadenamiento de métodos (method chaining) es el patrón de aplicar múltiples métodos en secuencia sobre el resultado del método anterior, sin variables intermedias. Es posible porque `map`, `filter`, `flatMap` y `flat` devuelven un nuevo array, sobre el que se puede volver a invocar cualquier método de `Array.prototype`. El resultado es un **pipeline de transformaciones** legible de izquierda a derecha.

```js
const datos = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5];

const resultado = datos
  .filter((n) => n > 3)          // [4, 5, 9, 6, 5, 5]
  .map((n) => n ** 2)            // [16, 25, 81, 36, 25, 25]
  .reduce((sum, n) => sum + n, 0); // 208
```

## Por qué funciona el encadenamiento

Cada método que devuelve un array crea un nuevo objeto `Array` que hereda de `Array.prototype`. Esto significa que el resultado de `filter()` tiene `map()`, que a su vez tiene `reduce()`, etc. Los métodos que devuelven `undefined` (`forEach`) o un valor no-array (`reduce`, `find`, `some`, `every`) rompen la cadena — deben ir al final.

| Método | Puede continuar la cadena |
|---|---|
| `map` | Sí (devuelve Array) |
| `filter` | Sí (devuelve Array) |
| `flatMap` | Sí (devuelve Array) |
| `flat` | Sí (devuelve Array) |
| `forEach` | No (devuelve `undefined`) |
| `reduce` / `reduceRight` | Solo si el resultado es un Array |
| `find` / `findLast` | No (devuelve elemento o `undefined`) |
| `findIndex` / `findLastIndex` | No (devuelve number) |
| `some` / `every` | No (devuelve boolean) |

## Ejemplos

### Pipeline completo: filtrar → transformar → reducir

```js
const empleados = [
  { nombre: "Ana", departamento: "IT", salario: 60000 },
  { nombre: "Luis", departamento: "HR", salario: 45000 },
  { nombre: "Eva", departamento: "IT", salario: 75000 },
  { nombre: "Tomás", departamento: "IT", salario: 55000 },
  { nombre: "Marta", departamento: "HR", salario: 50000 },
];

const promedioSalarioIT = empleados
  .filter((e) => e.departamento === "IT")           // [Ana, Eva, Tomás]
  .map((e) => e.salario)                             // [60000, 75000, 55000]
  .reduce((sum, s, _, arr) => sum + s / arr.length, 0); // 63333.33...
```

### Pipeline de datos: limpiar → normalizar → agrupar

```js
const rawUsuarios = [
  { id: 1, nombre: "  Ana García  ", activo: true },
  { id: 2, nombre: "luis pérez", activo: false },
  { id: 3, nombre: "Eva MARTIN", activo: true },
  { id: 4, nombre: "", activo: true },
];

const usuariosActivos = rawUsuarios
  .filter((u) => u.activo && u.nombre.trim())
  .map((u) => ({
    ...u,
    nombre: u.nombre.trim().toLowerCase().replace(/\b\w/g, (c) => c.toUpperCase()),
  }))
  .sort((a, b) => a.nombre.localeCompare(b.nombre));

// [{id:3, nombre:"Eva Martin",...}, {id:1, nombre:"Ana García",...}]
// (sort no es funcional puro — muta el array que devuelve el map, pero no el original)
```

### Usar `flatMap` para evitar un array intermedio

```js
// Con map + flat (dos arrays intermedios):
const resultado1 = pedidos
  .map((p) => p.items)      // [[item1, item2], [item3], [item4, item5]]
  .flat()                    // [item1, item2, item3, item4, item5]
  .filter((item) => item.disponible);

// Con flatMap (un array intermedio menos):
const resultado2 = pedidos
  .flatMap((p) => p.items)  // [item1, item2, item3, item4, item5]
  .filter((item) => item.disponible);
```

### Pipeline con `reduce` al final para agregar

```js
const ventas = [
  { producto: "Laptop", región: "Norte", monto: 1200 },
  { producto: "Mouse", región: "Sur", monto: 25 },
  { producto: "Laptop", región: "Norte", monto: 1200 },
  { producto: "Teclado", región: "Norte", monto: 80 },
];

const resumenNorte = ventas
  .filter((v) => v.región === "Norte")
  .reduce((acc, v) => {
    acc[v.producto] = (acc[v.producto] ?? 0) + v.monto;
    return acc;
  }, {});
// {Laptop: 2400, Teclado: 80}
```

## Orden importa: `filter` antes de `map`

El orden en que se colocan los métodos afecta tanto la corrección como el rendimiento:

```js
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// MENOS eficiente: map primero (transforma 10 elementos, luego filtra)
arr.map((n) => n ** 2).filter((n) => n > 25);
// map: 10 operaciones → filter sobre 10 elementos

// MÁS eficiente: filter primero (filtra 10, transforma solo los que pasan)
arr.filter((n) => n > 5).map((n) => n ** 2);
// filter: 10 comparaciones → map sobre 5 elementos → 5 operaciones
```

La regla general: si una transformación no cambia el criterio de filtrado, hacer `filter` antes de `map`.

## El problema del rendimiento con arrays grandes

Cada método en la cadena crea un array intermedio y recorre todos los elementos. Para `n` métodos encadenados sobre un array de tamaño `m`, el trabajo total es `O(n * m)` con `n - 1` arrays intermedios:

```js
// 3 pasadas completas, 2 arrays intermedios:
datos
  .filter(condición1)   // pasada 1 → array intermedio A
  .map(transformar)     // pasada 2 → array intermedio B
  .filter(condición2);  // pasada 3 → resultado final
```

Para arrays de tamaño normal (< 10,000 elementos), esto es completamente aceptable. Para millones de elementos, considerar:

**Opción 1: bucle único con lógica combinada**

```js
const resultado = [];
for (const x of datos) {
  if (condición1(x)) {
    const transformado = transformar(x);
    if (condición2(transformado)) resultado.push(transformado);
  }
}
// Una pasada, sin arrays intermedios
```

**Opción 2: `reduce` para combinar todo en una pasada**

```js
const resultado = datos.reduce((acc, x) => {
  if (condición1(x)) {
    const t = transformar(x);
    if (condición2(t)) acc.push(t);
  }
  return acc;
}, []);
```

**Opción 3: generadores con evaluación perezosa** (para casos muy extremos)

```js
function* pipeline(datos) {
  for (const x of datos) {
    if (condición1(x)) {
      const t = transformar(x);
      if (condición2(t)) yield t;
    }
  }
}
```

## Legibilidad: variables intermedias con nombre

Cuando el pipeline es largo o complejo, asignar resultados intermedios a variables mejora la legibilidad y facilita el debugging:

```js
// Cadena larga — difícil de debuggear
const resultado = datos
  .filter(esVálido)
  .map(normalizar)
  .flatMap(expandir)
  .filter(cumpleUmbral)
  .reduce(agregar, {});

// Con variables intermedias — fácil de inspeccionar cada paso
const válidos = datos.filter(esVálido);
const normalizados = válidos.map(normalizar);
const expandidos = normalizados.flatMap(expandir);
const filtrados = expandidos.filter(cumpleUmbral);
const resultado2 = filtrados.reduce(agregar, {});
```

La versión con variables intermedias permite poner `console.log` o un breakpoint entre cualquier par de pasos.

## Cómo funciona por dentro

El encadenamiento no tiene magia especial en el motor — es simplemente la ejecución secuencial de llamadas a métodos. No hay evaluación perezosa: cuando se ejecuta `arr.filter(...).map(...).reduce(...)`, JavaScript:

1. Ejecuta `filter(...)` completamente → crea el array A.
2. Ejecuta `map(...)` sobre A completamente → crea el array B.
3. Ejecuta `reduce(...)` sobre B completamente → devuelve el valor final.

Cada paso espera a que el anterior termine. No hay fusión de operaciones (loop fusion) ni evaluación perezosa en el engine V8 estándar para métodos de array nativos.

> [!tip] Buenas prácticas
> - Poner `filter` antes de `map` para reducir el trabajo de transformación.
> - Usar `flatMap` cuando el siguiente paso sería `map` seguido de `flat(1)`.
> - Para pipelines largos y legibles, formatear un método por línea con sangría consistente.
> - Poner `reduce` al final — convierte el array en un valor escalar, terminando la cadena.
> - Nombrar las funciones del callback en lugar de usar lambdas inline para que el pipeline sea auto-documentado: `.filter(esActivo).map(normalizar).reduce(agruparPorCiudad, {})`.

> [!warning] Errores comunes
> - **Encadenar después de `forEach`:** `forEach` devuelve `undefined` — intentar `.forEach(...).map(...)` lanza `TypeError: Cannot read property 'map' of undefined`.
> - **Encadenar después de `reduce` sin verificar el tipo:** si `reduce` devuelve un número o string, no tiene los métodos de array.
> - **Ignorar el rendimiento en datos masivos:** el encadenamiento es elegante pero no es lazy. Para millones de registros, medir antes de optimizar.
> - **Mutar objetos dentro del pipeline:** si el callback de `map` muta los objetos del array original (shallow reference), los cambios se propagan de formas inesperadas. Crear siempre copias con spread.
> - **Asumir que `sort` no muta:** `Array.prototype.sort` muta el array sobre el que se llama. En un pipeline, `datos.filter(...).sort(...)` muta el array de salida de `filter`, no el original — pero si ese array es el mismo objeto, puede haber efectos no esperados.

## Notas relacionadas

- [[index|Métodos de Array Funcionales — Índice]]
- [[02 map]]
- [[03 filter]]
- [[04 reduce]]
- [[11 flatMap]]
- [[10 flat]]
