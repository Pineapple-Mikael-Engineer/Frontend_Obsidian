---
title: Creación y Propiedades — sintaxis literal y descriptores
aliases:
  - object properties
  - property descriptors
  - defineProperty
tags:
  - javascript
  - api/concepto
  - objetos
objeto: Object
tipo: concepto
draft: false
---

# Creación y Propiedades

> [!definicion]
> Un objeto literal se crea con la sintaxis `{ clave: valor, … }`. Cada propiedad queda registrada internamente como un **descriptor de propiedad** — una estructura que controla `value`, `writable`, `enumerable` y `configurable` (propiedad de datos) o `get`, `set`, `enumerable` y `configurable` (propiedad de acceso). Las claves son strings o Symbols; cualquier otro tipo se convierte a string.

```js
const usuario = {
  nombre: "Ana",          // primitivo string
  edad: 30,               // primitivo number
  activo: true,           // primitivo boolean
  direccion: {            // objeto anidado
    ciudad: "Madrid",
  },
  etiquetas: ["admin"],   // array
  saludar() {             // método (shorthand)
    return `Hola, ${this.nombre}`;
  },
};
```

## Tipos de valores permitidos

Cualquier valor JavaScript: primitivos (`string`, `number`, `boolean`, `null`, `undefined`, `symbol`, `bigint`), objetos, arrays, funciones (métodos), otras referencias de objeto, e incluso `undefined` explícito. Un método es simplemente una propiedad cuyo valor es una función.

## `Object.defineProperty` — control fino

```js
const config = {};

Object.defineProperty(config, "MAX_RETRIES", {
  value: 3,
  writable: false,      // no se puede reasignar
  enumerable: false,    // no aparece en Object.keys ni for...in
  configurable: false,  // no se puede reconfigurar ni eliminar
});

console.log(config.MAX_RETRIES);  // → 3
console.log(Object.keys(config)); // → []   (no-enumerable)
config.MAX_RETRIES = 99;          // silenciado en modo no-estricto
// "use strict" → TypeError: Cannot assign to read only property
```

### Tabla de atributos del descriptor

| Atributo | Tipo | Defecto en `defineProperty` | Efecto |
|----------|------|:---------------------------:|--------|
| `value` | any | `undefined` | Valor almacenado |
| `writable` | boolean | `false` | Permite reasignar con `=` |
| `enumerable` | boolean | `false` | Aparece en `for...in`, `Object.keys` |
| `configurable` | boolean | `false` | Permite `delete` y redefinir el descriptor |
| `get` | function | `undefined` | Getter (excluye `value`/`writable`) |
| `set` | function | `undefined` | Setter (excluye `value`/`writable`) |

> [!warning] Defecto silencioso en literal vs `defineProperty`
> Al crear una propiedad en un literal `{}`, los tres flags (`writable`, `enumerable`, `configurable`) son `true` por defecto. Con `Object.defineProperty`, el defecto es `false` para los tres. Confundir los defectos produce propiedades más restrictivas de lo esperado.

## `Object.defineProperties` — múltiples a la vez

```js
const vector = {};

Object.defineProperties(vector, {
  x: { value: 0, writable: true, enumerable: true, configurable: true },
  y: { value: 0, writable: true, enumerable: true, configurable: true },
  magnitud: {
    get() { return Math.hypot(this.x, this.y); },
    enumerable: true,
    configurable: true,
  },
});

vector.x = 3;
vector.y = 4;
console.log(vector.magnitud); // → 5
```

## `Object.getOwnPropertyDescriptor` — leer el descriptor

```js
const obj = { nombre: "Ana" };
console.log(Object.getOwnPropertyDescriptor(obj, "nombre"));
// → { value: "Ana", writable: true, enumerable: true, configurable: true }

// Para leer todos los descriptores:
console.log(Object.getOwnPropertyDescriptors(obj));
```

`getOwnPropertyDescriptors` es útil para clonar con descriptores completos: `Object.create(Object.getPrototypeOf(src), Object.getOwnPropertyDescriptors(src))`.

## Propiedades de datos vs propiedades de acceso

Las dos familias son mutuamente excluyentes en el descriptor:

- **Datos:** tienen `value` y `writable`. La lectura devuelve `value` directamente.
- **Acceso:** tienen `get` y/o `set`. La lectura invoca `get()`; la escritura invoca `set(valor)`. No almacenan un valor en el slot.

Intentar definir `value` y `get` en el mismo descriptor lanza `TypeError`.

## Cómo funciona por dentro

El motor mantiene una tabla de propiedades por objeto. Cada entrada de esa tabla es un descriptor. Cuando se ejecuta `obj.prop`, el motor invoca `[[Get]]` que busca primero en las propiedades propias (la tabla) y luego sube por `[[Prototype]]`. Cuando se asigna `obj.prop = x`, invoca `[[Set]]`, que comprueba `writable` (en datos) o llama a `set` (en acceso) antes de modificar cualquier valor.

> [!tip] Buenas prácticas
> Usar `Object.defineProperty` para constantes de configuración que no deben ser sobreescritas ni iteradas. Para propiedades normales, la sintaxis literal con flags `true` es la opción correcta.

> [!warning] Errores comunes
> - Olvidar que `enumerable: false` hace que la propiedad no aparezca en `Object.keys`, `Object.values`, `Object.entries` ni `JSON.stringify`.
> - Declarar una propiedad como `configurable: false` y luego intentar redefinir su descriptor con `defineProperty` → `TypeError: Cannot redefine property`.
> - En modo no estricto, la escritura sobre una propiedad `writable: false` falla silenciosamente; en modo estricto lanza `TypeError`.

## Notas relacionadas

- [[06 Getters y Setters]] — propiedades de acceso definidas en literal
- [[07 Recorrer Propiedades (keys, values, entries)]] — cómo `enumerable` afecta la iteración
- [[08 Comprobar y Eliminar (in, hasOwnProperty, delete)]] — cómo `configurable` afecta `delete`
- [[02 Prototipos/index | Prototipos]] — la cadena de prototipos y `[[Get]]`
