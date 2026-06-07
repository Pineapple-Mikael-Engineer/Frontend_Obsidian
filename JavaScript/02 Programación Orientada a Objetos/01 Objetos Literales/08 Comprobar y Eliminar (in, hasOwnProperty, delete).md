---
title: Comprobar y Eliminar Propiedades — in, hasOwn, delete
aliases:
  - in operator
  - hasOwnProperty
  - Object.hasOwn
  - delete operator
  - property existence
tags:
  - javascript
  - api/concepto
  - objetos
objeto: Object
tipo: concepto
draft: false
---

# Comprobar y Eliminar (in, hasOwnProperty, delete)

> [!definicion]
> JavaScript ofrece tres mecanismos para comprobar si una propiedad existe en un objeto: el operador `in` (recorre la cadena de prototipos), `Object.hasOwn` (solo propiedades propias, ES2022) y el método heredado `obj.hasOwnProperty`. El operador `delete` elimina una propiedad propia si su descriptor marca `configurable: true`.

```js
const obj = { nombre: "Ana", edad: 30 };

console.log("nombre"   in obj);              // → true  (propia)
console.log("toString" in obj);              // → true  (heredada de Object.prototype)
console.log(Object.hasOwn(obj, "nombre"));   // → true
console.log(Object.hasOwn(obj, "toString")); // → false (no es propia)

delete obj.edad;
console.log(obj); // → { nombre: "Ana" }
```

## Operador `in`

```js
"prop" in obj
```

Devuelve `true` si la propiedad existe en el objeto **o en cualquier nodo de su cadena de prototipos**, independientemente de `enumerable`. También detecta propiedades con valor `undefined`:

```js
const config = { timeout: undefined };

console.log("timeout" in config);            // → true  (existe, aunque su valor sea undefined)
console.log(config.timeout !== undefined);   // → false (confunde existencia con valor)
```

El operador `in` aparece además en el cabezal de `for...in`. Para comprobar solo propiedades propias, no es el candidato correcto.

## `Object.hasOwn` (ES2022) — recomendado

```js
Object.hasOwn(obj, "prop")
```

Comprueba que la propiedad sea **propia** del objeto (no heredada). Es el reemplazo moderno de `obj.hasOwnProperty("prop")` con dos ventajas:

1. Funciona en objetos creados con `Object.create(null)` (que no tienen `hasOwnProperty` en su prototipo).
2. No se ve afectado si el objeto tiene una propiedad propia llamada `hasOwnProperty` que sobreescriba el método.

```js
// Objeto sin prototipo
const mapa = Object.create(null);
mapa.clave = "valor";

console.log(Object.hasOwn(mapa, "clave"));   // → true
console.log(mapa.hasOwnProperty("clave"));   // → TypeError: mapa.hasOwnProperty is not a function

// Objeto que sobreescribe hasOwnProperty
const trampa = { hasOwnProperty() { return false; } };
console.log(trampa.hasOwnProperty("hasOwnProperty")); // → false (mentira)
console.log(Object.hasOwn(trampa, "hasOwnProperty")); // → true  (correcto)
```

## `obj.hasOwnProperty` — uso legacy

```js
obj.hasOwnProperty("prop")
```

Equivalente a `Object.hasOwn` para la mayoría de objetos ordinarios, pero con las limitaciones descritas arriba. En código nuevo, preferir `Object.hasOwn`. En código existente, la llamada segura es `Object.prototype.hasOwnProperty.call(obj, "prop")` para esquivar la sobreescritura.

## Tabla comparativa

| Mecanismo | Propias | Heredadas | Cuidados |
|-----------|:-------:|:---------:|---------|
| `"prop" in obj` | Sí | Sí | Devuelve `true` para `toString`, `constructor`, etc. |
| `obj.hasOwnProperty("prop")` | Sí | No | Falla en `Object.create(null)`; puede sobreescribirse |
| `Object.hasOwn(obj, "prop")` | Sí | No | Requiere ES2022; la opción segura y moderna |
| `delete obj.prop` | Solo propias configurables | — | No funciona en variables ni propiedades no-configurables |

## Operador `delete`

```js
delete obj.prop
```

Elimina la propiedad propia `prop` del objeto. Devuelve `true` en todos los casos excepto cuando la propiedad existe y es no-configurable (devuelve `false` en modo no estricto; lanza `TypeError` en modo estricto).

```js
const obj2 = { a: 1, b: 2 };

console.log(delete obj2.a);        // → true
console.log(obj2);                 // → { b: 2 }

console.log(delete obj2.noExiste); // → true (no hace nada, pero no lanza error)

// Propiedad no-configurable
Object.defineProperty(obj2, "fijo", { value: 99, configurable: false });
console.log(delete obj2.fijo);     // → false (modo no estricto)
// "use strict" → TypeError: Cannot delete property 'fijo'
```

### `delete` no elimina variables

```js
var x = 10;
let y = 20;

console.log(delete x); // → false (no elimina variables globales declaradas con var)
console.log(delete y); // → false
console.log(x);        // → 10
```

Las variables declaradas con `var`, `let` y `const` tienen `configurable: false` en su descriptor de entorno, por lo que `delete` no las afecta.

## Receta: eliminar clave de forma inmutable (sin mutar el original)

```js
const usuario = { nombre: "Ana", password: "secret", edad: 30 };

// Con rest destructuring (ES2018):
const { password, ...sinPassword } = usuario;
console.log(sinPassword);  // → { nombre: "Ana", edad: 30 }
console.log(usuario);      // → no mutado: { nombre: "Ana", password: "secret", edad: 30 }

// Con Object.fromEntries:
const sinPassword2 = Object.fromEntries(
  Object.entries(usuario).filter(([k]) => k !== "password")
);
```

## Receta: comprobar antes de acceder a propiedad anidada

```js
function obtenerCiudad(datos) {
  if (Object.hasOwn(datos, "direccion") && Object.hasOwn(datos.direccion, "ciudad")) {
    return datos.direccion.ciudad;
  }
  return null;
}

// Alternativa moderna con optional chaining:
const ciudad = datos?.direccion?.ciudad ?? null;
```

## Cómo funciona por dentro

- **`in`**: invoca `[[HasProperty]]` en el objeto. Si no encuentra la propiedad propia (vía `[[GetOwnProperty]]`), sube recursivamente por `[[Prototype]]` hasta llegar a `null`. El costo es proporcional a la profundidad de la cadena.
- **`Object.hasOwn` / `hasOwnProperty`**: invoca `[[GetOwnProperty]]` directamente, sin recorrer la cadena. Devuelve el descriptor si existe, `undefined` si no.
- **`delete`**: invoca `[[Delete]]` que consulta el descriptor de la propiedad. Si `configurable` es `false`, devuelve `false` (o lanza en modo estricto). Si `configurable` es `true`, elimina el slot de la propiedad de la tabla interna del objeto y devuelve `true`. Nótese que `delete` no libera la memoria del valor inmediatamente si hay otras referencias vivas a ese valor.

> [!tip] Buenas prácticas
> - Usar `Object.hasOwn` en lugar de `hasOwnProperty` en código nuevo.
> - Preferir el rest destructuring `const { clave, ...resto } = obj` para eliminar propiedades sin mutar el original.
> - Usar `in` solo cuando se necesita comprobar incluyendo la cadena de prototipos (por ejemplo, verificar si un objeto implementa un protocolo via Symbol).

> [!warning] Errores comunes
> - Comprobar existencia con `obj.prop !== undefined`: falla cuando la propiedad existe pero su valor es explícitamente `undefined`. Usar `Object.hasOwn` o `in` en su lugar.
> - Asumir que `delete` en propiedades de prototipos afecta a las instancias: `delete obj.prop` solo actúa sobre propiedades propias de `obj`. Las propiedades heredadas no se tocan.
> - Usar `delete` sobre propiedades de arrays para "eliminar" elementos: deja un slot vacío (`empty`), no recorre los índices. Usar `splice` para eliminar elementos de array correctamente.

## Notas relacionadas

- [[01 Creación y Propiedades]] — descriptores y el flag `configurable`
- [[07 Recorrer Propiedades (keys, values, entries)]] — iterar propiedades propias vs heredadas
- [[01 Programación Procedimental/04 Operadores/09 Operador in.md | Operador in]] — detalle completo
- [[02 Prototipos/03 Propiedades Propias vs Heredadas.md | Propiedades Propias vs Heredadas]] — cómo distinguirlas en la cadena de prototipos
