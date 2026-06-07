---
title: Paso por Valor vs Referencia — Cómo JS pasa argumentos a funciones
aliases:
  - pass by value
  - pass by reference
  - paso por valor
  - paso por referencia
  - paso de argumentos JS
tags:
  - javascript
  - api/concepto
  - funciones
objeto: global
tipo: concepto
draft: false
---

# Paso por Valor vs Referencia

> [!definicion]
> JavaScript pasa los argumentos **siempre por valor**. Para **primitivos** (number, string, boolean, null, undefined, symbol, bigint), ese valor es el dato en sí: la función recibe una copia y no puede afectar la variable del llamador. Para **objetos** (incluyendo arrays y funciones), el valor es una **referencia** (dirección de memoria) al objeto en el heap: la función puede mutar las propiedades del objeto, pero no puede redirigir la variable del llamador a otro objeto.

```js
// Primitivo — la copia no afecta al original
function triplicar(n) {
  n = n * 3; // modifica la copia local
}
let precio = 10;
triplicar(precio);
console.log(precio); // → 10  (sin cambios)

// Objeto — mutar propiedades SÍ afecta al original
function aplicarDescuento(producto) {
  producto.precio *= 0.9; // muta la propiedad
}
const laptop = { nombre: "Laptop", precio: 1000 };
aplicarDescuento(laptop);
console.log(laptop.precio); // → 900  (mutado)
```

## El modelo mental correcto

JavaScript no es "paso por referencia" en el sentido de C++ (donde la función puede reasignar la variable del llamador). Es mejor describirlo como **"paso por valor de la referencia"** o **"call by sharing"**:

```js
function intentarReasignar(obj) {
  obj = { nuevo: true }; // reasigna el parámetro local — no afecta al exterior
}

const original = { dato: 42 };
intentarReasignar(original);
console.log(original); // → { dato: 42 }  — sin cambios
// El parámetro obj apuntaba al mismo objeto que original,
// pero reasignar obj solo cambia a qué apunta obj localmente.
```

## Diagrama del mecanismo

Al llamar `f(obj)`:

```
Llamador:          Función f:
  original  ──→  [ { dato: 42 } ]  ←──  obj (parámetro local)
                      heap

Mutar obj.dato → cambia el objeto en el heap → original.dato también cambia
Reasignar obj = {} → obj apunta a otro lugar → original sigue apuntando al original
```

## Tabla resumen

| Tipo de argumento | La función puede mutar | La función puede reasignar la variable del llamador |
|---|---|---|
| Primitivo | no (es una copia) | no |
| Objeto / Array | sí (muta el heap) | no |
| Referencia a función | no aplica | no |

## Recetas: evitar mutación accidental

**Copia superficial con spread** — suficiente para objetos sin propiedades anidadas:

```js
function aplicarDescuentoSeguro(producto) {
  const copia = { ...producto }; // nuevo objeto en el heap
  copia.precio *= 0.9;
  return copia;
}

const laptop = { nombre: "Laptop", precio: 1000 };
const laptopConDescuento = aplicarDescuentoSeguro(laptop);
console.log(laptop.precio);           // → 1000  (intacto)
console.log(laptopConDescuento.precio); // → 900
```

**Copia profunda con `structuredClone`** — para objetos anidados:

```js
function procesarConfig(config) {
  const copia = structuredClone(config); // deep copy real (ES2022+)
  copia.db.puerto = 5432;
  return copia;
}

const cfg = { db: { host: "localhost", puerto: 3000 } };
const cfgProcesada = procesarConfig(cfg);
console.log(cfg.db.puerto);           // → 3000  (intacto)
console.log(cfgProcesada.db.puerto);  // → 5432
```

**`Object.assign`** — alternativa a spread para copia superficial:

```js
const copia = Object.assign({}, original);
```

## Cuándo la mutación es intencional

La mutación directa es válida cuando el patrón de la función es transformar un objeto in-place, como en acumulación o construcción incremental:

```js
// Patrón builder — mutación intencional
function agregarCampo(schema, nombre, tipo) {
  schema.campos.push({ nombre, tipo });
  return schema; // retorna el mismo objeto (fluent interface)
}

const schema = { tabla: "usuarios", campos: [] };
agregarCampo(schema, "id", "integer");
agregarCampo(schema, "email", "varchar");
// schema.campos = [{ nombre: "id", tipo: "integer" }, ...]
```

En estos casos, la convención es documentarlo explícitamente y nombrar la función de forma que indique modificación in-place (`agregar`, `actualizar`, `limpiar`).

## Strings y arrays: el caso que confunde

Los **strings** son primitivos en JavaScript: inmutables y pasados por valor. No existe `string.muta()`.

Los **arrays** son objetos y se pasan por referencia al objeto array. Métodos como `push`, `pop`, `splice` mutan el array original:

```js
function agregarItem(lista, item) {
  lista.push(item); // muta el array del llamador
}

const carrito = ["manzana", "pera"];
agregarItem(carrito, "naranja");
console.log(carrito); // → ["manzana", "pera", "naranja"]

// Para no mutar, usar concat o spread:
function agregarItemSin(lista, item) {
  return [...lista, item]; // nuevo array
}
```

## Cómo funciona por dentro

La **call stack** almacena los frames de ejecución. Cada frame contiene los bindings locales. Al pasar un argumento:

- **Primitivo**: el motor copia el valor bit a bit en el binding del parámetro. El frame del llamador y el frame de la función tienen copias independientes.
- **Objeto**: el motor copia la **dirección de memoria** (un número, típicamente 32 o 64 bits) en el binding del parámetro. Ambos frames apuntan al mismo objeto en el **heap**. Mutar el objeto a través de esa dirección afecta a todos los que la compartan; reasignar el binding local solo cambia qué dirección almacena ese binding local.

> [!tip] Buenas prácticas
> - Documentar si una función muta sus argumentos o devuelve un nuevo valor. El nombre puede indicarlo: `ordenar` vs `ordenado`, `filtrar` vs `filtrado`.
> - Preferir funciones que devuelven nuevos valores (estilo funcional) para evitar acoplamiento implícito a través de la mutación.
> - Usar `structuredClone` para deep copies; el spread (`{ ...obj }`) solo copia el primer nivel.

> [!warning] Errores comunes
> **Asumir que los primitivos se comparten** — el error opuesto:
> ```js
> function incrementar(n) { n++; }
> let contador = 0;
> incrementar(contador);
> console.log(contador); // → 0, no 1
> // Si se necesita modificar un contador, retornar el valor o usar un objeto wrapper
> ```
> **Spread superficial con objetos anidados** — el spread copia solo el primer nivel; las propiedades anidadas siguen compartiendo referencia:
> ```js
> const original = { a: 1, nested: { b: 2 } };
> const copia = { ...original };
> copia.nested.b = 99;
> console.log(original.nested.b); // → 99  ← la copia superficial no protegió nested
> // Solución: structuredClone(original)
> ```

## Notas relacionadas

- [[07 Funciones (básico)/index | Funciones (básico)]]
- [[JavaScript/01 Programación Procedimental/03 Tipos de Datos/index | Tipos de Datos]]
- [[JavaScript/03 Programación Funcional/02 Funciones de Orden Superior/index | Funciones de Orden Superior]]
- [[JavaScript/03 Programación Funcional/05 Inmutabilidad Práctica/01 Spread Operator | Spread Operator]]
- [[JavaScript/03 Programación Funcional/05 Inmutabilidad Práctica/02 Object.assign() | Object.assign()]]
