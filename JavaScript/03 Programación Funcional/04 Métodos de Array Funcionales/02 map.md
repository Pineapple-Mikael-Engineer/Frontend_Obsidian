---
title: "map"
aliases:
  - Array map
  - array.map
tags:
  - javascript
  - api/metodo
  - arrays
draft: false
---

# map

> [!definicion]
> `Array.prototype.map` aplica un callback a cada elemento del array y devuelve un **nuevo array** con los valores retornados por el callback, en el mismo orden y con el mismo tamaño. El array original no se modifica. Es el método canónico para **transformar** cada elemento en otro valor.

```js
const precios = [10, 25, 40];
const conIVA = precios.map((precio) => precio * 1.21);
// [12.1, 30.25, 48.4]

// El original no cambia
console.log(precios); // [10, 25, 40]
```

## Firma del método

| Parámetro del callback | Tipo | Descripción |
|---|---|---|
| `elemento` | any | Valor del elemento actual |
| `índice` | number | Posición (0-based) |
| `array` | Array | Referencia al array original |

| Aspecto | Detalle |
|---|---|
| Valor de retorno del método | Nuevo `Array` del mismo `length` |
| Muta el array | No |
| Argumento `thisArg` | Opcional; establece el `this` del callback |
| Salta huecos (array sparse) | Sí; los preserva como huecos en el resultado |

## Ejemplos

### Extraer una propiedad de un array de objetos

```js
const usuarios = [
  { id: 1, nombre: "Ana" },
  { id: 2, nombre: "Luis" },
  { id: 3, nombre: "Eva" },
];

const nombres = usuarios.map((u) => u.nombre);
// ["Ana", "Luis", "Eva"]

const ids = usuarios.map((u) => u.id);
// [1, 2, 3]
```

### Transformar objetos

```js
const productos = [
  { nombre: "Laptop", precio: 1000 },
  { nombre: "Mouse", precio: 25 },
];

const conDescuento = productos.map((p) => ({
  ...p,
  precioFinal: p.precio * 0.9,
  enOferta: true,
}));
// [{nombre: "Laptop", precio: 1000, precioFinal: 900, enOferta: true}, ...]
```

### Convertir tipos

```js
const strings = ["1", "2", "3", "4"];
const nums = strings.map((s) => Number(s));
// [1, 2, 3, 4]

// Alternativa con función constructora (OJO con la trampa)
// NO hacer: strings.map(Number) — funciona por casualidad aquí,
// pero puede fallar con otras funciones que aceptan múltiples argumentos
```

### Usar el índice para generar valores dependientes de posición

```js
const letras = ["a", "b", "c", "d"];
const conPosicion = letras.map((letra, i) => `${i + 1}. ${letra}`);
// ["1. a", "2. b", "3. c", "4. d"]
```

## Recetas comunes

### Normalizar datos de una API

```js
const rawData = [
  { user_name: "ana_garcia", birth_year: 1990 },
  { user_name: "luis_perez", birth_year: 1985 },
];

const normalizado = rawData.map(({ user_name, birth_year }) => ({
  username: user_name.replace(/_/g, " "),
  age: new Date().getFullYear() - birth_year,
}));
```

### Generar nodos de React/HTML dinámicos (patrón común en frameworks)

```js
const items = ["inicio", "sobre mí", "contacto"];
const liElements = items.map((item, i) => `<li id="nav-${i}">${item}</li>`);
const html = `<ul>${liElements.join("")}</ul>`;
```

### Transformar claves de un objeto dentro de un array

```js
const camelToSnake = (str) => str.replace(/([A-Z])/g, "_$1").toLowerCase();

const registros = [{ firstName: "Ana", lastName: "García" }];
const snakeCase = registros.map((obj) =>
  Object.fromEntries(
    Object.entries(obj).map(([k, v]) => [camelToSnake(k), v])
  )
);
// [{first_name: "Ana", last_name: "García"}]
```

## La trampa clásica: `map(parseInt)`

```js
["1", "2", "3"].map(parseInt);
// Resultado: [1, NaN, NaN]
```

El problema es que `parseInt` acepta dos argumentos: `parseInt(string, radix)`. Cuando se pasa como callback de `map`, recibe `(valor, índice, array)`, por lo que el índice actúa como base (radix):

- `parseInt("1", 0)` → base 0 se trata como 10 → `1`
- `parseInt("2", 1)` → base 1 es inválida → `NaN`
- `parseInt("3", 2)` → base 2 binario, "3" no es dígito binario → `NaN`

La solución correcta es envolver en una función que solo pase el valor:

```js
["1", "2", "3"].map((s) => parseInt(s, 10));
// [1, 2, 3]

// O usar Number, que solo toma un argumento:
["1", "2", "3"].map(Number);
// [1, 2, 3]
```

Esta trampa aplica a cualquier función que use su segundo argumento para algo distinto al índice: `parseFloat` (no la sufre porque ignora el segundo arg), pero `Number` tampoco (un solo parámetro).

## Cómo funciona por dentro

`map` crea un nuevo array vacío del mismo `length` que el original. Luego itera desde índice `0` hasta `length - 1`. Para cada índice que sea propiedad propia del array, invoca `callback(array[i], i, array)` y almacena el valor de retorno en la misma posición del nuevo array. Los huecos (índices no existentes en arrays sparse) no se visitan y se preservan como huecos en el resultado. Al terminar, devuelve el nuevo array.

```js
// Implementación conceptual simplificada de map
Array.prototype.mapPropio = function(callback, thisArg) {
  const resultado = new Array(this.length);
  for (let i = 0; i < this.length; i++) {
    if (i in this) {                                    // salta huecos
      resultado[i] = callback.call(thisArg, this[i], i, this);
    }
  }
  return resultado;
};
```

Si el callback no tiene `return` explícito (función con llaves `{}`), devuelve `undefined`, y el nuevo array contendrá `undefined` en esa posición.

```js
// Bug frecuente: olvidar return en función con cuerpo
const doble = [1, 2, 3].map((n) => { n * 2 });
// [undefined, undefined, undefined] — falta el return

// Correcto con cuerpo de función:
const doble2 = [1, 2, 3].map((n) => { return n * 2; });
// [2, 4, 6]

// O con arrow function de expresión (sin llaves, return implícito):
const doble3 = [1, 2, 3].map((n) => n * 2);
// [2, 4, 6]
```

> [!tip] Buenas prácticas
> - Usar `map` cuando el resultado importa y necesitas un array del mismo tamaño. Si no usas el array resultado, usa `forEach`.
> - El callback de `map` debe ser una función pura (sin efectos secundarios). Mezclar efectos con `map` hace el código difícil de razonar.
> - Para transformar objetos, usar desestructuración y spread para mayor claridad: `items.map(({ id, name }) => ({ id, label: name }))`.
> - Envolver funciones multi-argumento en arrow functions al usarlas como callback para evitar la trampa del segundo argumento.
> - Cuando el callback es una transformación simple y sin efectos, se puede pasar directamente: `arr.map(String)`, `arr.map(Boolean)`.

> [!warning] Errores comunes
> - **Olvidar `return` en función con llaves:** produce un array de `undefined`. Arrow functions de expresión (sin `{}`) tienen return implícito.
> - **Pasar funciones multi-argumento directamente:** la trampa de `parseInt`, `Number` es seguro pero `parseInt` no. Envolver siempre en `(x) => fn(x)`.
> - **Usar `map` para efectos secundarios:** si el callback no retorna un valor útil, la herramienta correcta es `forEach`.
> - **Esperar que `map` mute el original:** `map` nunca muta — si necesitas modificar el original, usar una iteración explícita.
> - **`map` sobre objetos planos:** `map` solo existe en `Array`. Para transformar las entradas de un objeto, usar `Object.entries(obj).map(...).` y reconstruir con `Object.fromEntries`.

## Notas relacionadas

- [[index|Métodos de Array Funcionales — Índice]]
- [[01 forEach]]
- [[03 filter]]
- [[04 reduce]]
- [[11 flatMap]]
- [[12 Encadenamiento de Métodos]]
