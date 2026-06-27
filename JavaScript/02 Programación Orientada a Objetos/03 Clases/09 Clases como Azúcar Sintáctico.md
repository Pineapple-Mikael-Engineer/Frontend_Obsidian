---
title: Clases como Azúcar Sintáctico — Equivalencias con prototipos
aliases:
  - clases sugar syntax
  - class vs prototype
  - azúcar sintáctico JS
  - clases vs prototipos
tags:
  - javascript
  - api/concepto
  - clases
objeto: global
tipo: concepto
draft: false
order: 9
---

# Clases como Azúcar Sintáctico

> [!definicion]
> Las clases de ES6 no introducen un modelo de objetos nuevo. Son azúcar sintáctico sobre el sistema prototípico de JavaScript: el spec ECMAScript define cada construcción de clase en términos de operaciones sobre funciones y prototipos. No existe ningún tipo de objeto "clase" en el motor distinto del tipo "función".

```js
// Equivalencia directa:
class Punto {
  constructor(x, y) { this.x = x; this.y = y; }
  distancia() { return Math.hypot(this.x, this.y); }
  static origen() { return new Punto(0, 0); }
}

// Equivale a:
function Punto(x, y) { "use strict"; this.x = x; this.y = y; }
Object.defineProperty(Punto.prototype, "distancia", {
  value: function distancia() { return Math.hypot(this.x, this.y); },
  enumerable: false, configurable: true, writable: true,
});
Punto.origen = function origen() { return new Punto(0, 0); };

// Son funcionalmente iguales:
typeof Punto;                                 // → "function"
new Punto(3, 4).distancia();                  // → 5
Punto.origen() instanceof Punto;              // → true
```

## Tabla de equivalencias clase ↔ prototipo

| Construcción de clase | Equivalente prototípico |
|-----------------------|------------------------|
| `class Nombre { }` | `function Nombre() { "use strict"; }` |
| `constructor(args)` | Cuerpo de la función constructora |
| `metodo() { }` | `Object.defineProperty(Nombre.prototype, "metodo", { enumerable: false, ... })` |
| `static metodo() { }` | `Nombre.metodo = function() { }` |
| `static prop = val` | `Nombre.prop = val` (ejecutado al definir la clase) |
| `extends Padre` | `Nombre.prototype = Object.create(Padre.prototype)` + `Object.setPrototypeOf(Nombre, Padre)` |
| `super(args)` en constructor | `Padre.call(this, args)` (con semántica de new.target) |
| `super.metodo()` | `Padre.prototype.metodo.call(this)` |
| `get prop() { }` | `Object.defineProperty(..., { get: fn, enumerable: false, ... })` |

## Diferencias reales — lo que las clases añaden

Las clases no son solo cosmética. Introducen comportamientos que no tienen equivalente directo y limpio en las funciones constructoras:

**1. TDZ — no hoistea:**
```js
// Función constructora → hoistea completamente
const p = new Punto(1, 2); // OK
function Punto(x, y) { this.x = x; this.y = y; }

// Clase → TDZ
const q = new Figura(1, 2); // ReferenceError
class Figura { constructor(x, y) { this.x = x; this.y = y; } }
```

**2. Strict mode siempre activo:**
El cuerpo de clase corre en strict mode aunque el archivo no lo declare. Sin clases, hay que añadir `"use strict"` manualmente.

**3. Métodos no enumerables por defecto:**
```js
class A { metodo() {} }
function B() {}
B.prototype.metodo = function() {};

for (const k in new A()) console.log(k); // — nada
for (const k in new B()) console.log(k); // → "metodo"
```

**4. Campos privados `#campo`:**
No existe equivalente prototípico real. El mecanismo WeakMap-like que implementan los motores no es replicable con sintaxis prototípica sin perder la garantía de acceso exclusivo.

**5. `new.target`:**
```js
class Abstracta {
  constructor() {
    if (new.target === Abstracta) {
      throw new Error("Abstracta no puede instanciarse directamente");
    }
  }
}

class Concreta extends Abstracta {
  constructor() { super(); this.tipo = "concreta"; }
}

new Concreta().tipo; // → "concreta"
new Abstracta();     // Error: Abstracta no puede instanciarse directamente
```

`new.target` dentro del constructor apunta a la función con la que se llamó `new`. En la función constructora clásica no existe este mecanismo de forma nativa.

## Por qué importa la equivalencia

**Leer código legacy:** gran parte del código JavaScript anterior a ES6 (Node.js, jQuery, frameworks pre-React) usa funciones constructoras y `prototype` directamente. Entender la equivalencia permite leer y modificar ese código sin confusión.

**Depurar la cadena de prototipos:**
```js
class Animal {}
class Perro extends Animal {}

const d = new Perro();
Object.getPrototypeOf(d) === Perro.prototype;           // → true
Object.getPrototypeOf(Perro.prototype) === Animal.prototype; // → true
Object.getPrototypeOf(Perro) === Animal;                // → true (estáticos)
```

**Rendimiento:** los métodos en `prototype` son compartidos entre instancias — una sola función en memoria para cualquier número de objetos. Los class fields con arrow functions crean una copia por instancia.

## Dónde rompe la analogía

La equivalencia no es perfecta. El spec define comportamientos para `class` que requieren mecanismos internos que las funciones constructoras no pueden reproducir sin el motor:

- Los campos privados `#` requieren slots internos del motor — no se pueden emular con WeakMaps públicos sin perder la opacidad frente a herramientas de debuggeo y sin cierta diferencia de comportamiento.
- `super()` en un constructor de subclase no es exactamente `Padre.call(this)`: las clases derivadas requieren que el objeto sea creado por el constructor más derivado, no por `Object.create(Padre.prototype)`. El spec llama esto "[[Construct]] de clase derivada".
- `new.target` no existe en funciones constructoras pre-ES6.

> [!tip] Buenas prácticas
> - Inspeccionar la cadena de prototipos con `Object.getPrototypeOf` o `__proto__` en la consola para depurar problemas de herencia.
> - Conocer la equivalencia permite elegir conscientemente entre clase y función constructora en código que interopera con módulos legacy.
> - Preferir clases en código nuevo por las garantías adicionales (TDZ, strict mode, no-enumerable); usar la equivalencia prototípica para razonar sobre rendimiento y cadena de búsqueda.

> [!warning] Errores comunes
> - Asumir que "clases son solo azúcar" implica que son idénticas a las funciones constructoras — las diferencias listadas son reales y afectan el comportamiento observable.
> - Mezclar clases y prototipos manualmente en la misma jerarquía sin entender el orden de inicialización (fields vs constructor vs prototype).
> - Intentar replicar campos privados con `_convención` y asumir el mismo nivel de encapsulamiento — `_prop` es visible externamente y modificable; `#prop` no.

## Notas relacionadas

- [[index | Clases — índice]]
- [[01 Declaración de Clase]]
- [[08 Herencia (extends, super)]]
- [[02 Constructor]]
