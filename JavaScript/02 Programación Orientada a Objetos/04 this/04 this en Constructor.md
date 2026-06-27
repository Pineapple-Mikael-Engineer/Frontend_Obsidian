---
title: this en Constructor — New binding
aliases:
  - this en constructor
  - new binding
  - this con new
tags:
  - javascript
  - api/concepto
  - this
objeto: global
tipo: concepto
muta: false
asincrono: false
draft: false
order: 4
---

# `this` en Constructor — New binding

> [!definicion]
> Cuando una función (o clase) se invoca con `new`, se aplica el **new binding**: el motor crea un objeto nuevo con `[[Prototype]]` apuntando a `Fn.prototype`, establece ese objeto como `this` dentro del cuerpo de la función, y al finalizar lo devuelve automáticamente. Es el binding de mayor prioridad junto con `call`/`apply`/`bind`.

```js
function Producto(nombre, precio) {
  this.nombre = nombre;
  this.precio = precio;
}

const libro = new Producto("El nombre del viento", 18.5);
console.log(libro.nombre);  // → "El nombre del viento"
console.log(libro.precio);  // → 18.5
console.log(libro instanceof Producto); // → true
```

## El mismo mecanismo con `class`

Las clases son azúcar sintáctico sobre funciones constructoras y prototipos. `new Clase()` ejecuta exactamente los mismos pasos internos.

```js
class Carrito {
  constructor(usuario) {
    this.usuario = usuario;
    this.items = [];
  }

  agregar(item) {
    this.items.push(item);
    return this;
  }

  total() {
    return this.items.reduce((acc, i) => acc + i.precio, 0);
  }
}

const carrito = new Carrito("Ana");
carrito.agregar({ nombre: "Libro", precio: 18.5 });
carrito.total(); // → 18.5
```

## Retornar un objeto explícitamente: reemplaza `this`

Si el constructor devuelve un **objeto**, ese objeto reemplaza al `this` recién creado como resultado de `new`. Si devuelve un primitivo (o nada), el resultado sigue siendo el `this` original.

```js
function ConRetornoObjeto() {
  this.a = 1;
  return { b: 2 };  // objeto explícito → reemplaza this
}

function ConRetornoPrimitivo() {
  this.a = 1;
  return 99;         // primitivo → se ignora, retorna this
}

const x = new ConRetornoObjeto();
console.log(x);     // → { b: 2 }  — this perdido

const y = new ConRetornoPrimitivo();
console.log(y);     // → ConRetornoPrimitivo { a: 1 }
```

## `new.target` — detectar si se usó `new`

`new.target` es una meta-propiedad disponible dentro de funciones y constructores. Cuando la función se invoca con `new`, `new.target` apunta a la función/clase invocada. En llamadas normales sin `new`, vale `undefined`.

```js
function Persona(nombre) {
  if (!new.target) {
    // Llamada sin new — comportamiento seguro: redirigir o lanzar
    throw new TypeError("Usa new para crear una Persona");
  }
  this.nombre = nombre;
}

Persona("Ana");        // → TypeError
const p = new Persona("Ana"); // → ok
```

### Clase abstracta con `new.target`

```js
class Forma {
  constructor() {
    if (new.target === Forma) {
      throw new Error("Clase abstracta — instanciar solo subclases");
    }
    this.tipo = new.target.name; // nombre de la subclase concreta
  }
}

class Circulo extends Forma {
  constructor(radio) {
    super();
    this.radio = radio;
  }
}

new Forma();            // → Error: Clase abstracta
const c = new Circulo(5);
console.log(c.tipo);   // → "Circulo"
```

## Cómo funciona por dentro

> [!info]
> Los pasos que ejecuta `new F(args)` según el spec:
> 1. Crear un objeto vacío `obj = {}`.
> 2. Establecer `obj.[[Prototype]] = F.prototype`.
> 3. Llamar a `F` con `this = obj` (new binding) y los `args`.
> 4. Si `F` devuelve un objeto, ese objeto es el resultado; si no, el resultado es `obj`.
>
> Las clases añaden la restricción de que el constructor de una subclase **debe** llamar a `super()` antes de acceder a `this` — el objeto no se crea en la subclase sino en la superclase más alta de la cadena.

## Buenas prácticas

> [!tip]
> Usar `new.target` para hacer constructoras defensivas — detectar llamadas accidentales sin `new` antes de que contaminen el objeto global (en modo no estricto) o lancen un `TypeError` críptico. En clases ES6, el motor lanza automáticamente si se intenta llamar a un `constructor` de clase sin `new`, por lo que la guarda manual es más útil en funciones constructoras pre-ES6.

## Errores comunes

> [!warning]
> **Devolver un objeto accidentalmente en un constructor** elimina el `this` creado y rompre el resultado de `instanceof`. Es un bug silencioso: `new MiClase() instanceof MiClase` devuelve `false` si el constructor devuelve un objeto literal externo. Revisar que los constructores nunca retornen objetos explícitamente (salvo en el patrón Singleton intencionado).

## Notas relacionadas

- [[index|this — Contexto de invocación]] — prioridad de bindings
- [[02 Prototipos/05 Funciones Constructoras (new, this)|Funciones Constructoras (new, this)]] — herencia prototípica clásica
- [[03 Clases/02 Constructor|Constructor]] — constructor en clases ES6
- [[03 Clases/08 Herencia (extends, super)|Herencia (extends, super)]] — `super()` y creación del `this` en subclases
