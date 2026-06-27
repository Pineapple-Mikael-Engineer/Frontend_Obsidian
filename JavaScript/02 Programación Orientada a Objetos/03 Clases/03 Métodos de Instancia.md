---
title: Métodos de Instancia — Métodos compartidos en el prototype
aliases:
  - instance methods
  - métodos de clase JS
  - prototype methods
tags:
  - javascript
  - api/concepto
  - clases
objeto: global
tipo: concepto
draft: false
order: 3
---

# Métodos de Instancia

> [!definicion]
> Los métodos declarados directamente en el cuerpo de una clase (sin `static`) se añaden a `Clase.prototype` y son compartidos por todas las instancias. Se definen sin la palabra clave `function` y no son enumerables, a diferencia de métodos añadidos manualmente a `prototype`.

```js
class Pila {
  constructor() { this._items = []; }

  push(item) { this._items.push(item); }
  pop()      { return this._items.pop(); }
  get size() { return this._items.length; }
}

const p = new Pila();
p.push(1); p.push(2);
p.pop();  // → 2
p.size;   // → 1

// El método vive en el prototype, no en la instancia:
Object.hasOwn(p, "push"); // → false
"push" in Pila.prototype; // → true
```

## No enumerables — diferencia respecto a prototype manual

```js
// Método de clase → no enumerable
class Foo {
  metodo() {}
}
Object.getOwnPropertyDescriptor(Foo.prototype, "metodo");
// → { value: [Function: metodo], writable: true, enumerable: false, configurable: true }

// Método manual en prototype → enumerable por defecto
function Bar() {}
Bar.prototype.metodo = function() {};
Object.getOwnPropertyDescriptor(Bar.prototype, "metodo");
// → { value: [Function], writable: true, enumerable: true, configurable: true }

// Consecuencia:
const foo = new Foo(), bar = new Bar();
for (const k in foo) console.log(k); // nada — "metodo" no es enumerable
for (const k in bar) console.log(k); // "metodo"
```

## La trampa del `this` perdido

Los métodos de clase se añaden al `prototype`, por lo que el binding de `this` depende del sitio de llamada. Al extraer el método, `this` se pierde.

```js
class Contador {
  constructor() { this.n = 0; }
  incrementar() { this.n++; }
}

const c = new Contador();
c.incrementar();   // this = c  → OK, c.n = 1
const inc = c.incrementar;
inc();             // this = undefined (strict mode) → TypeError: Cannot set properties of undefined

// Soluciones:
const inc2 = c.incrementar.bind(c);
inc2(); // OK

// O usar un campo de clase con arrow (ver nota 04 Propiedades de Instancia)
```

## Métodos `async` y generadores

La sintaxis shorthand de métodos acepta `async` y generadores sin cambios estructurales:

```js
class Repositorio {
  async buscar(id) {
    const res = await fetch(`/api/items/${id}`);
    return res.json();
  }

  *[Symbol.iterator]() {
    yield* this._items;
  }
}
```

## Receta: clase iterable con `Symbol.iterator`

Implementar `[Symbol.iterator]()` convierte la instancia en iterable nativo (compatible con `for...of`, spread, destructuring).

```js
class Rango {
  constructor(inicio, fin) {
    this.inicio = inicio;
    this.fin = fin;
  }

  *[Symbol.iterator]() {
    for (let i = this.inicio; i <= this.fin; i++) {
      yield i;
    }
  }
}

const r = new Rango(1, 5);
[...r];                           // → [1, 2, 3, 4, 5]
for (const n of r) console.log(n); // 1 2 3 4 5
```

## Receta: método que retorna `this` para encadenamiento

El patrón fluent interface / method chaining se logra retornando `this` de cada método mutador:

```js
class Constructor {
  #config = {};

  nombre(val)    { this.#config.nombre = val;    return this; }
  precio(val)    { this.#config.precio = val;    return this; }
  categoria(val) { this.#config.categoria = val; return this; }
  build()        { return { ...this.#config }; }
}

const producto = new Constructor()
  .nombre("Auriculares")
  .precio(89.99)
  .categoria("audio")
  .build();
// → { nombre: "Auriculares", precio: 89.99, categoria: "audio" }
```

## Cómo funciona por dentro

Al evaluar el cuerpo de una clase, el motor procesa cada método y llama internamente a:

```js
Object.defineProperty(Clase.prototype, "nombreMetodo", {
  value: function nombreMetodo(...args) { /* cuerpo */ },
  enumerable:   false,
  configurable: true,
  writable:     true,
});
```

`enumerable: false` distingue los métodos de clase de la asignación directa `Clase.prototype.metodo = fn`. `configurable: true` permite que subclases los sobreescriban.

> [!tip] Buenas prácticas
> - Para métodos que se pasarán como callbacks, usar un campo de clase con arrow function (ver [[04 Propiedades de Instancia]]) si el binding correcto de `this` es crítico.
> - Métodos en `prototype` son la opción eficiente por defecto: una copia compartida entre todas las instancias, no duplicados por objeto.
> - Nombrar los métodos como verbos o frases verbales (`calcular`, `obtenerNombre`, `validarEntrada`).

> [!warning] Errores comunes
> - Extraer un método y pasarlo como callback a `setTimeout`, `addEventListener` o `Array.forEach` pierde `this`. Usar `.bind(this)` o arrow wrapper.
> - Asumir que un método de clase es enumerable (para `Object.keys`, `JSON.stringify`, `for...in`) — no lo es; solo los datos propios de la instancia lo son.
> - Usar `function` keyword dentro del cuerpo de clase: `metodo: function() {}` — SyntaxError. El cuerpo de clase solo admite la forma shorthand.

## Notas relacionadas

- [[index | Clases — índice]]
- [[04 Propiedades de Instancia]]
- [[07 Getters y Setters]]
- [[09 Clases como Azúcar Sintáctico]]
