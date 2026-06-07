---
title: Herencia — extends y super en clases JS
aliases:
  - herencia clases JS
  - extends super
  - class inheritance
  - subclase JS
tags:
  - javascript
  - api/concepto
  - clases
objeto: global
tipo: concepto
draft: false
---

# Herencia (`extends`, `super`)

> [!definicion]
> `class Hija extends Padre` establece automáticamente la cadena de prototipos de instancias (`Hija.prototype → Padre.prototype`) y la cadena de clases constructoras (`Hija → Padre`), habilitando herencia de métodos de instancia y de métodos estáticos respectivamente. `super()` en el constructor de la subclase llama al constructor del padre y es obligatorio antes de usar `this`.

```js
class Animal {
  constructor(nombre) { this.nombre = nombre; }
  hablar() { return `${this.nombre} hace un sonido.`; }
  toString() { return `Animal(${this.nombre})`; }
}

class Perro extends Animal {
  constructor(nombre, raza) {
    super(nombre);        // obligatorio antes de this
    this.raza = raza;
  }
  hablar() {
    return `${this.nombre} ladra.`; // sobreescribe el método del padre
  }
}

const d = new Perro("Rex", "Labrador");
d.hablar();              // → "Rex ladra."
d.toString();            // → "Animal(Rex)"  — heredado de Animal
d instanceof Perro;      // → true
d instanceof Animal;     // → true
```

## `super()` — obligatorio en constructor de subclase

En una subclase, el objeto `this` no existe hasta que `super()` haya sido llamado. Intentar acceder a `this` antes lanza `ReferenceError`.

```js
class Vehiculo {
  constructor(marca) { this.marca = marca; }
}

class Auto extends Vehiculo {
  constructor(marca, modelo) {
    // this.modelo = modelo; // ReferenceError: Must call super constructor before accessing 'this'
    super(marca);
    this.modelo = modelo; // OK — super() ya fue llamado
  }
}

new Auto("Toyota", "Corolla").modelo; // → "Corolla"
```

## `super.metodo()` — delegar al padre

Dentro de los métodos de instancia de la subclase, `super.metodo()` llama al método del padre aunque la subclase lo haya sobreescrito.

```js
class Logger {
  log(msg) { console.log(`[INFO] ${msg}`); }
}

class ErrorLogger extends Logger {
  log(msg) {
    super.log(msg);                      // invoca Logger.prototype.log
    console.error(`[ERROR] ${msg}`);
  }
}

new ErrorLogger().log("algo falló");
// → [INFO] algo falló
// → [ERROR] algo falló
```

## Sobreescritura (override)

Un método en `Hija` con el mismo nombre que uno en `Padre` lo sombrea en la cadena de prototipos. La búsqueda de propiedades se detiene en `Hija.prototype` antes de llegar a `Padre.prototype`.

```js
class Forma {
  area()      { return 0; }
  toString()  { return `Forma(area=${this.area()})`; }
}

class Cuadrado extends Forma {
  constructor(lado) { super(); this.lado = lado; }
  area() { return this.lado ** 2; } // override
}

class Triangulo extends Forma {
  constructor(b, h) { super(); this.b = b; this.h = h; }
  area() { return (this.b * this.h) / 2; } // override
}

new Cuadrado(4).toString();   // → "Forma(area=16)"  — toString hereda pero llama area() de Cuadrado
new Triangulo(6, 3).area();   // → 9
```

## Herencia de métodos estáticos

Los métodos estáticos se heredan porque el `[[Prototype]]` de `Hija` (la función constructora) apunta a `Padre`.

```js
class Modelo {
  static from(datos) { return new this(datos); }
}

class Usuario extends Modelo {
  constructor(datos) { super(); this.datos = datos; }
}

Usuario.from({ nombre: "Ana" }).datos; // → { nombre: "Ana" }
// new this() en el método estático usa this = Usuario
```

## `extends` con expresiones — mixins

`extends` acepta cualquier expresión que evalúe a una función constructora, lo que permite componer comportamientos con mixins:

```js
const Serializable = (Base) => class extends Base {
  serialize()   { return JSON.stringify(this); }
  static parse(json) { return Object.assign(new this(), JSON.parse(json)); }
};

const Validable = (Base) => class extends Base {
  validar(schema) { return schema.every(k => k in this); }
};

class Entidad {
  constructor(id) { this.id = id; }
}

class Producto extends Serializable(Validable(Entidad)) {
  constructor(id, nombre, precio) {
    super(id);
    this.nombre  = nombre;
    this.precio  = precio;
  }
}

const p = new Producto(1, "Silla", 120);
p.serialize();                           // → '{"id":1,"nombre":"Silla","precio":120}'
p.validar(["id", "nombre", "precio"]);   // → true
```

## `instanceof` con herencia

`instanceof` recorre la cadena de prototipos — retorna `true` para la clase propia y para todos sus ancestros.

```js
class A {}
class B extends A {}
class C extends B {}

const c = new C();
c instanceof C; // → true
c instanceof B; // → true
c instanceof A; // → true
c instanceof Object; // → true
```

## Cómo funciona por dentro

`extends` realiza dos operaciones distintas:

```js
// 1. Herencia de instancias — encadena prototipos de instancias:
Hija.prototype = Object.create(Padre.prototype);
Hija.prototype.constructor = Hija;

// 2. Herencia de estáticos — encadena las funciones constructoras:
Object.setPrototypeOf(Hija, Padre);

// Resultado:
Hija.prototype.__proto__ === Padre.prototype; // → true  (instancias)
Hija.__proto__           === Padre;           // → true  (estáticos)
```

`super()` en el constructor equivale conceptualmente a `Padre.call(this, ...args)` pero con la semántica de new.target intacta para campos de clase y [[09 Clases como Azúcar Sintáctico]].

> [!tip] Buenas prácticas
> - Llamar `super()` en la primera línea del constructor de la subclase, antes de cualquier `this.prop`.
> - Al sobreescribir un método, considerar si `super.metodo()` debe invocarse — evita perder la lógica del padre (patrón "extender, no reemplazar").
> - Para herencia de comportamiento sin jerarquía rígida, preferir mixins sobre herencia profunda.
> - La herencia debe modelar una relación "es un tipo de" genuina — profundidades superiores a 2-3 niveles suelen indicar un diseño que se beneficia de composición.

> [!warning] Errores comunes
> - Omitir `super()` en el constructor de la subclase — `ReferenceError` al acceder a `this`.
> - Olvidar que `super.metodo()` usa `super` como referencia al `prototype` del padre, no como `this` del padre — `super.metodo()` invoca el método con `this` apuntando a la instancia de la subclase.
> - Redefinir un método estático en la subclase sin llamar `super.metodoEstatico()` cuando la lógica del padre era necesaria.
> - Pasar una expresión no constructora a `extends` (null, un primitivo, un objeto literal sin `[[Construct]]`) — lanza `TypeError`.

## Notas relacionadas

- [[index | Clases — índice]]
- [[02 Constructor]]
- [[03 Métodos de Instancia]]
- [[05 Métodos y Propiedades Estáticas]]
- [[09 Clases como Azúcar Sintáctico]]
