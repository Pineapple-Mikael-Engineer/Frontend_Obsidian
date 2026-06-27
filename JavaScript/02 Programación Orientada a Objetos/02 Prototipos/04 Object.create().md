---
title: Object.create() — Crear objetos con prototipo explícito
aliases:
  - Object.create
  - create
tags:
  - javascript
  - api/metodo
  - prototipos
  - oop
objeto: Object
tipo: metodo
retorna: object
muta: false
asincrono: false
draft: false
order: 4
---

# `Object.create()`

> [!definicion]
> `Object.create(proto, descriptors?)` crea un nuevo objeto vacío y establece su `[[Prototype]]` a `proto`. Es la operación primitiva de creación con prototipo: más directa que `new`, sin constructor, con control total sobre el nodo de herencia. Si `proto` es `null`, el objeto resultante no tiene cadena de prototipos en absoluto.

```js
const animal = {
  hablar() { return `${this.nombre} habla`; },
};

const perro = Object.create(animal);
perro.nombre = "Rex";
perro.hablar(); // "Rex habla"

Object.getPrototypeOf(perro) === animal; // true
```

## Firma completa

```js
Object.create(proto)
Object.create(proto, descriptors)
```

- `proto` — objeto o `null`. Cualquier otro tipo lanza `TypeError`.
- `descriptors` — objeto de descriptores de propiedades, igual que el segundo argumento de `Object.defineProperties`. Opcional.

## `Object.create(null)` — Mapa puro

El objeto resultante **no tiene prototipo**. No hereda `toString`, `hasOwnProperty`, `valueOf`, `constructor`, ni ninguna propiedad de `Object.prototype`. Ideal para diccionarios donde las claves son arbitrarias y no deben colisionar con métodos heredados.

```js
const mapa = Object.create(null);
mapa["toString"] = "un dato real"; // no colisiona con nada
mapa["constructor"] = "otro dato";

"toString" in mapa; // true — es propia, no heredada
Object.getPrototypeOf(mapa); // null

// No tiene hasOwnProperty:
mapa.hasOwnProperty; // undefined
// Usar Object.hasOwn en su lugar:
Object.hasOwn(mapa, "toString"); // true
```

## Segundo argumento: descriptores de propiedades

```js
const obj = Object.create(Object.prototype, {
  x: { value: 10, writable: true, enumerable: true, configurable: true },
  y: { value: 20, writable: false, enumerable: true, configurable: false },
});

obj.x; // 10
obj.y; // 20
obj.y = 99; // silencioso en no-strict, TypeError en strict — no es writable
```

Equivale a crear el objeto y luego llamar a `Object.defineProperties(obj, descriptors)`.

## Herencia sin constructor: patrón de objetos literales

```js
const Forma = {
  area() { return 0; },
  perimetro() { return 0; },
  describe() { return `Forma con área ${this.area()}`; },
};

const Rectangulo = Object.create(Forma);
Rectangulo.init = function(w, h) {
  this.w = w;
  this.h = h;
  return this;
};
Rectangulo.area = function() { return this.w * this.h; };
Rectangulo.perimetro = function() { return 2 * (this.w + this.h); };

const r = Object.create(Rectangulo).init(3, 4);
r.area();      // 12
r.describe();  // "Forma con área 12" — delega a Forma.describe → this.area() → 12
```

## Comparación con `new Clase()`

| Aspecto | `Object.create(proto)` | `new F()` |
|---|---|---|
| Requiere constructor | no | sí |
| Prototipo del resultado | `proto` | `F.prototype` |
| Ejecuta código de inicialización | no (manual) | sí (cuerpo de `F`) |
| Retorna | objeto | objeto (o lo que retorne `F`) |
| Complejidad de la cadena | explícita | implícita |

## Receta: clonar con prototipo preservado

```js
function clonar(obj) {
  return Object.assign(Object.create(Object.getPrototypeOf(obj)), obj);
}

// El clon tiene el mismo prototipo que el original y una copia de las propiedades propias enumerables
```

## Receta: mixin-like sin herencia múltiple real

```js
// Combinar comportamientos de múltiples objetos en el prototipo del nuevo objeto
const proto = Object.assign({}, Serializable, Validable, Auditable);
const entidad = Object.create(proto);
```

> [!warning]
> `Object.assign({}, proto1, proto2)` copia las propiedades propias enumerables al nuevo objeto plano. Las propiedades no enumerables y los descriptores complejos (getters, setters) **no se copian fielmente**. Si se necesita una copia fiel de descriptores, usar `Object.getOwnPropertyDescriptors` con `Object.defineProperties`.

## Cómo funciona por dentro

`Object.create(proto)` aloca un nuevo objeto en el heap del motor (en V8, crea un nuevo `JSObject`) y establece su campo `[[Prototype]]` a `proto` antes de que el objeto sea visible para el código de usuario. No invoca ningún constructor. Es la operación primitiva sobre la que `new F()` está construida: el primer paso de `new` es equivalente a `Object.create(F.prototype)`.

> [!tip]
> `Object.create` es la forma preferida cuando se quiere control explícito sobre el prototipo sin la maquinaria de un constructor. Para jerarquías complejas de múltiples instancias, `class` es más legible; `Object.create` brilla para prototipos únicos (singletons de comportamiento) y mapas puros con `null`.

## Notas relacionadas

- [[01 Cadena de Prototipos]]
- [[02 getPrototypeOf y setPrototypeOf]]
- [[05 Funciones Constructoras (new, this)]]
- [[06 Herencia Prototípica]]
