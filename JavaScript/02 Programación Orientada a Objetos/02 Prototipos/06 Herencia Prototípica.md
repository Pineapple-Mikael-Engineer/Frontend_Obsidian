---
title: Herencia Prototípica — Extender constructoras antes de class
aliases:
  - herencia prototípica
  - prototypal inheritance
  - herencia con constructoras
tags:
  - javascript
  - api/concepto
  - prototipos
  - oop
objeto: Object
tipo: concepto
muta: false
asincrono: false
draft: false
---

# Herencia Prototípica

> [!definicion]
> La **herencia prototípica** consiste en establecer la cadena `[[Prototype]]` de una función constructora hija de modo que apunte al prototipo de la constructora padre. Las instancias de la hija delegan al prototipo de la hija, que a su vez delega al prototipo del padre. No hay copia de métodos: es delegación pura. El patrón pre-ES6 requiere tres pasos manuales: llamar al constructor padre, enlazar los prototipos y restaurar `constructor`.

```js
function Animal(nombre) {
  this.nombre = nombre;
}
Animal.prototype.hablar = function() {
  return `${this.nombre} hace un sonido`;
};

function Perro(nombre, raza) {
  Animal.call(this, nombre);     // 1. Inicializar propiedades del padre en this
  this.raza = raza;
}
Perro.prototype = Object.create(Animal.prototype); // 2. Enlazar cadena
Perro.prototype.constructor = Perro;               // 3. Restaurar constructor

Perro.prototype.ladrar = function() {
  return `${this.nombre} dice: Guau`;
};

const d = new Perro("Rex", "Labrador");
d.ladrar(); // "Rex dice: Guau"
d.hablar(); // "Rex hace un sonido" — delegado a Animal.prototype
d instanceof Perro;  // true
d instanceof Animal; // true
```

## Los tres pasos explicados

### 1. `Animal.call(this, nombre)` — Inicializar propiedades del padre

Invoca el constructor de `Animal` con `this` apuntando a la nueva instancia de `Perro`. Sin este paso, las instancias de `Perro` no tendrían las propiedades propias inicializadas por `Animal` (`this.nombre`, etc.).

```js
// Sin Animal.call — las propiedades del padre no se inicializan:
function PerroMal(nombre, raza) {
  this.raza = raza; // no hay this.nombre
}
PerroMal.prototype = Object.create(Animal.prototype);
const mal = new PerroMal("Rex", "Lab");
mal.nombre;  // undefined — Animal.call no se invocó
```

### 2. `Perro.prototype = Object.create(Animal.prototype)` — Enlazar la cadena

```
instancia de Perro
    [[Prototype]] → Perro.prototype
                       [[Prototype]] → Animal.prototype
                                           [[Prototype]] → Object.prototype
                                                               [[Prototype]] → null
```

Por qué `Object.create(Animal.prototype)` y no `new Animal()`:

- `new Animal()` ejecuta el cuerpo de `Animal`, que puede requerir argumentos y producir efectos secundarios.
- `Object.create(Animal.prototype)` crea un objeto vacío con `[[Prototype]] = Animal.prototype`, sin ejecutar código de `Animal`.

```js
// Versión correcta:
Perro.prototype = Object.create(Animal.prototype);

// Versión problemática:
Perro.prototype = new Animal(); // ejecuta Animal() sin argumentos → nombre = undefined
```

### 3. `Perro.prototype.constructor = Perro` — Restaurar `constructor`

Al reemplazar `Perro.prototype` con un objeto nuevo, ese objeto hereda `constructor` de `Animal.prototype`, que apunta a `Animal`. Se debe restaurar explícitamente.

```js
// Sin restaurar:
Perro.prototype = Object.create(Animal.prototype);
Perro.prototype.constructor; // Animal ← incorrecto

// Con restaurar:
Perro.prototype.constructor = Perro;
Perro.prototype.constructor; // Perro ← correcto

// Efecto en el código de usuario:
const d = new Perro("Rex", "Lab");
d.constructor === Perro; // true — ahora correcto
```

## Sobrescribir métodos del padre (override)

```js
Perro.prototype.hablar = function() {
  // Llamar al método del padre explícitamente:
  const base = Animal.prototype.hablar.call(this);
  return `${base} (pero ladra)`;
};

d.hablar(); // "Rex hace un sonido (pero ladra)"
```

No hay `super` en el patrón clásico. El acceso al método padre se hace referenciando `PadreConstructora.prototype.metodo.call(this, ...)` directamente.

## Patrón completo con tres niveles

```js
// Nivel 1
function Ser(vivo) {
  this.vivo = vivo;
}
Ser.prototype.estado = function() { return this.vivo ? "vivo" : "inerte"; };

// Nivel 2
function Animal(nombre) {
  Ser.call(this, true);
  this.nombre = nombre;
}
Animal.prototype = Object.create(Ser.prototype);
Animal.prototype.constructor = Animal;
Animal.prototype.hablar = function() { return `${this.nombre} habla`; };

// Nivel 3
function Perro(nombre, raza) {
  Animal.call(this, nombre);
  this.raza = raza;
}
Perro.prototype = Object.create(Animal.prototype);
Perro.prototype.constructor = Perro;
Perro.prototype.ladrar = function() { return "Guau"; };

const d = new Perro("Rex", "Lab");
d.estado();  // "vivo"   — Ser.prototype
d.hablar();  // "Rex habla" — Animal.prototype
d.ladrar();  // "Guau"   — Perro.prototype
d instanceof Perro;  // true
d instanceof Animal; // true
d instanceof Ser;    // true
```

## Cómo funciona el recorrido de métodos

Al leer `d.hablar()`:
1. Motor busca `hablar` en `d` (instancia de Perro) → no está.
2. Sube a `Perro.prototype` → no está (a menos que se haya sobreescrito).
3. Sube a `Animal.prototype` → está → ejecuta con `this = d`.

El mecanismo es el algoritmo `[[Get]]` de la [[01 Cadena de Prototipos]] aplicado recursivamente.

## Lo que `class` hace automáticamente

El patrón clásico de 3 pasos es verboso y propenso a errores. `class` los encapsula:

```js
class Animal {
  constructor(nombre) { this.nombre = nombre; }
  hablar() { return `${this.nombre} habla`; }
}

class Perro extends Animal {
  constructor(nombre, raza) {
    super(nombre); // equivale a Animal.call(this, nombre)
    this.raza = raza;
  }
  ladrar() { return "Guau"; }
}
```

Internamente, `class Perro extends Animal` realiza exactamente los 3 pasos del patrón clásico: `Perro.prototype = Object.create(Animal.prototype)`, restaura `constructor` y configura la cadena entre las funciones mismas (`Object.setPrototypeOf(Perro, Animal)`).

> [!tip]
> El patrón clásico es relevante para leer código legacy (pre-2015), depurar transpilaciones de Babel/TypeScript y entender con precisión qué ocurre cuando se usa `class`. Para código nuevo, usar `class extends`.

> [!warning]
> Olvidar el paso 2 (`Object.create`) significa que `Perro.prototype` apunta a `Object.prototype` en lugar de `Animal.prototype`, rompiendo la cadena. `instanceof Animal` retorna `false`. Olvidar el paso 3 no rompe la funcionalidad inmediata, pero confunde a cualquier código que dependa de `obj.constructor`.

## Notas relacionadas

- [[01 Cadena de Prototipos]]
- [[04 Object.create()]]
- [[05 Funciones Constructoras (new, this)]]
- [[03 Clases/08 Herencia (extends, super) | Herencia con extends y super]]
- [[03 Clases/09 Clases como Azúcar Sintáctico | Clases como Azúcar Sintáctico]]
