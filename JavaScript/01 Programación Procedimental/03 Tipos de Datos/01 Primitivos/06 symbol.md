---
title: symbol — Identificadores únicos e irrepetibles
aliases:
  - symbol JS
  - Symbol
  - tipo symbol
tags:
  - javascript
  - api/concepto
  - tipos
objeto: Symbol
tipo: concepto
draft: false
order: 6
---

# symbol

> [!definicion]
> Un `symbol` es un valor primitivo **único e irrepetible**. Dos llamadas a `Symbol()` con la misma descripción producen symbols distintos. Su uso principal es crear claves de propiedades garantizadas únicas en objetos, evitando colisiones con otras librerías o código de terceros.

```js
typeof Symbol()         // → "symbol"
typeof Symbol("desc")   // → "symbol"

Symbol("id") === Symbol("id")   // → false  (son distintos)
```

## Creación

```js
const s1 = Symbol();           // sin descripción
const s2 = Symbol("nombre");   // con descripción (solo para depuración)

console.log(s2.toString());    // → "Symbol(nombre)"
console.log(s2.description);   // → "nombre"
```

La descripción es únicamente de depuración; no afecta la unicidad ni la identidad del symbol.

## Uso como clave de propiedad

El caso de uso central es evitar colisiones de nombres de propiedades:

```js
const ID = Symbol("id");
const NOMBRE = Symbol("nombre");

const usuario = {
  [ID]: 42,
  [NOMBRE]: "Ana",
  nombre: "otro nombre visible"  // no colisiona con el symbol NOMBRE
};

console.log(usuario[ID]);     // → 42
console.log(usuario[NOMBRE]); // → "Ana"
console.log(usuario.nombre);  // → "otro nombre visible"
```

Las propiedades symbol **no aparecen** en `for...in`, `Object.keys()`, `Object.values()` ni `JSON.stringify()`. Solo son accesibles con `Object.getOwnPropertySymbols()`.

```js
for (const k in usuario) {
  console.log(k);  // solo imprime "nombre"; ID y NOMBRE son invisibles
}

Object.keys(usuario);               // → ["nombre"]
Object.getOwnPropertySymbols(usuario);  // → [Symbol(id), Symbol(nombre)]
```

## Registro global: `Symbol.for()` y `Symbol.keyFor()`

`Symbol.for(clave)` accede a un registro global compartido: si ya existe un symbol con esa clave, lo retorna; si no, lo crea.

```js
const s1 = Symbol.for("app.id");
const s2 = Symbol.for("app.id");

s1 === s2   // → true  (mismo del registro global)

Symbol.keyFor(s1)   // → "app.id"
Symbol.keyFor(Symbol("local"))   // → undefined  (no está en el registro)
```

Útil para compartir symbols entre módulos o iframes sin exportar el mismo objeto.

## Well-known symbols (symbols conocidos)

JavaScript expone symbols predefinidos en `Symbol.*` que controlan comportamientos internos del lenguaje:

| Symbol | Controla |
|---|---|
| `Symbol.iterator` | Protocolo iterable (`for...of`, spread) |
| `Symbol.toPrimitive` | Conversion a primitivo (en contextos `+`, template, etc.) |
| `Symbol.hasInstance` | Comportamiento de `instanceof` |
| `Symbol.toStringTag` | Etiqueta en `Object.prototype.toString.call()` |
| `Symbol.asyncIterator` | Protocolo de iteracion asincrona (`for await...of`) |
| `Symbol.species` | Constructor para metodos que crean instancias derivadas |

```js
// Symbol.iterator: hacer un objeto iterable
const rango = {
  from: 1,
  to: 5,
  [Symbol.iterator]() {
    let actual = this.from;
    const fin = this.to;
    return {
      next() {
        return actual <= fin
          ? { value: actual++, done: false }
          : { done: true };
      }
    };
  }
};

[...rango]    // → [1, 2, 3, 4, 5]
```

```js
// Symbol.toPrimitive: controlar conversión implícita
const dinero = {
  cantidad: 100,
  moneda: "USD",
  [Symbol.toPrimitive](hint) {
    if (hint === "number") return this.cantidad;
    if (hint === "string") return `${this.cantidad} ${this.moneda}`;
    return this.cantidad;
  }
};

+dinero          // → 100
`${dinero}`      // → "100 USD"
dinero + 50      // → 150
```

## Pseudo-privacidad (antes de campos privados `#`)

Antes de ES2022 y los campos privados con `#`, los symbols se usaban para "ocultar" propiedades internas de objetos en librerías. No es privacidad real (se pueden listar con `getOwnPropertySymbols`), pero impide accesos accidentales.

```js
const _estado = Symbol("estado");

class Conexion {
  constructor() {
    this[_estado] = "cerrada";
  }
  abrir() { this[_estado] = "abierta"; }
  get estado() { return this[_estado]; }
}

const c = new Conexion();
c.estado          // → "cerrada"
c[_estado]        // accesible si se tiene la referencia al symbol
```

> [!tip] Buenas prácticas
> - Usar symbols como claves de propiedades de extensión en objetos de terceros para no contaminar el namespace.
> - Usar `Symbol.for()` cuando el symbol debe ser compartido entre módulos distintos.
> - Para privacidad real, usar campos privados `#` (ES2022).

> [!warning] Errores comunes
> - `Symbol("id") === Symbol("id")` → `false`: cada llamada produce un symbol nuevo.
> - `JSON.stringify({[Symbol("x")]: 1})` → `"{}"`: los symbols se ignoran en la serialización.
> - `Symbol()` no se puede llamar con `new`: `new Symbol()` → `TypeError`.
> - No convertir a string implícitamente: `"id: " + Symbol("x")` → `TypeError`. Usar `String(s)` o `.toString()` explícitamente.

## Notas relacionadas

- [[01 Primitivos/index|Primitivos]]
- [[03 instanceof|instanceof]] — `Symbol.hasInstance` personaliza `instanceof`
- [[02 typeof|typeof]]
