---
title: for...in — Iterar propiedades enumerables
aliases:
  - for in
  - for...in loop
tags:
  - javascript
  - api/concepto
  - control-flujo
objeto: global
tipo: concepto
draft: false
---

# for...in

> [!definicion]
> `for...in` itera las **propiedades enumerables** de un objeto, incluyendo las heredadas de la cadena de prototipos. Produce las **claves** (strings), no los valores. Sintaxis: `for (const clave in objeto) { cuerpo }`.

```js
const persona = { nombre: "Ana", edad: 30, ciudad: "Madrid" };

for (const clave in persona) {
  console.log(`${clave}: ${persona[clave]}`);
}
// nombre: Ana
// edad: 30
// ciudad: Madrid
```

## Cómo funciona por dentro

El motor construye la lista de propiedades enumerables de toda la cadena de prototipos del objeto (no solo las propias) y las itera. El atributo `enumerable` de cada propiedad determina si aparece o no. Las propiedades nativas del prototipo (`toString`, `hasOwnProperty`, etc.) tienen `enumerable: false` y no aparecen. Las propiedades añadidas manualmente al prototipo con `enumerable: true` sí aparecen.

El orden de iteración sigue el orden de inserción para claves string no-enteras en motores modernos. Para claves que se parsean como índices enteros (como los de un array), el orden puede variar — el spec no lo garantiza, aunque en V8 se ordenan numéricamente.

## El patrón seguro: `Object.hasOwn`

Para iterar solo propiedades propias (no heredadas), filtrar con `Object.hasOwn`:

```js
function Vehiculo() { this.tipo = "vehículo"; }
Vehiculo.prototype.motor = "combustión";

const coche = new Vehiculo();
coche.marca = "Toyota";

// Sin filtro — incluye propiedades heredadas
for (const k in coche) {
  console.log(k); // "tipo", "marca", "motor"
}

// Con filtro — solo propias
for (const k in coche) {
  if (Object.hasOwn(coche, k)) {
    console.log(k); // "tipo", "marca"
  }
}
```

`Object.hasOwn(obj, key)` es la forma moderna; `obj.hasOwnProperty(key)` es equivalente pero puede ser sobreescrita si el objeto no hereda de `Object.prototype`.

## Por qué no usar `for...in` en arrays

Tres razones:

1. Las claves se producen como **strings** (`"0"`, `"1"`), no números.
2. Si alguien añade propiedades al `Array.prototype` (una librería antigua, por ejemplo), esas propiedades aparecen en la iteración.
3. El orden no está garantizado por la especificación para claves enteras.

```js
Array.prototype.suma = function() { return this.reduce((a, b) => a + b, 0); };

const nums = [10, 20, 30];
for (const k in nums) {
  console.log(k, typeof k); // "0" string, "1" string, "2" string, "suma" string
}
```

Para arrays, usar [[05 for...of]] o [[03 for Clásico]].

## Alternativas modernas para objetos

Cuando no se necesitan propiedades heredadas, los métodos estáticos de `Object` son más explícitos y producen arrays directamente encadenables:

```js
const config = { host: "localhost", port: 3000, debug: true };

Object.keys(config).forEach(k => console.log(k));           // claves propias
Object.values(config).forEach(v => console.log(v));         // valores propios
Object.entries(config).forEach(([k, v]) => console.log(k, v)); // pares [clave, valor]

// Con for...of
for (const [clave, valor] of Object.entries(config)) {
  console.log(`${clave} = ${valor}`);
}
```

## Cuándo sí es útil `for...in`

- Depuración rápida: listar todas las propiedades (propias y heredadas) de un objeto desconocido.
- Cuando genuinamente se necesitan las propiedades enumerables **heredadas** — un caso raro pero legítimo en metaprogramación o frameworks que usan la cadena de prototipos como mecanismo de herencia de configuración.
- Código que debe ejecutarse en entornos muy antiguos sin soporte para `Object.entries`.

> [!tip] Buenas prácticas
> - Siempre acompañar `for...in` con `Object.hasOwn(obj, key)` a menos que explícitamente se quieran las propiedades heredadas.
> - Para arrays, usar `for...of` o `for` clásico.
> - Para objetos planos (plain objects), preferir `Object.entries()` + `for...of` — es más predecible y encadenable.

> [!warning] Errores comunes
> **Usar `for...in` en arrays** — produce claves string y puede incluir propiedades del prototipo si alguna librería lo ha modificado. Es un bug clásico en código JavaScript antiguo.
> **Asumir orden de iteración** — la especificación no garantiza orden para propiedades enteras. Aunque V8 ordena numéricamente en la práctica, no es seguro depender de ello.
> **Omitir `Object.hasOwn`** — en herencia prototípica, las propiedades del prototipo aparecen y pueden causar comportamientos inesperados.

## Notas relacionadas

- [[06 Bucles/index | Bucles]]
- [[05 for...of]]
- [[03 for Clásico]]
- [[JavaScript/02 Programación Orientada a Objetos/02 Prototipos/index | Prototipos]]
- [[JavaScript/02 Programación Orientada a Objetos/01 Objetos Literales/07 Recorrer Propiedades (keys, values, entries) | Recorrer Propiedades (keys, values, entries)]]
