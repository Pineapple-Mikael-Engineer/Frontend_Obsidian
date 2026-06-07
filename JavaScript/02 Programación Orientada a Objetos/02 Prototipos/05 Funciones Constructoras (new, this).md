---
title: Funciones Constructoras — new, this y F.prototype
aliases:
  - funciones constructoras
  - constructor function
  - new operator
  - new F()
tags:
  - javascript
  - api/concepto
  - prototipos
  - oop
objeto: Function
tipo: concepto
muta: false
asincrono: false
draft: false
---

# Funciones Constructoras (`new`, `this`)

> [!definicion]
> Una **función constructora** es una función ordinaria invocada con `new`. El operador `new` crea un objeto cuyo `[[Prototype]]` es `F.prototype`, llama a `F` con ese objeto como `this`, y retorna el objeto resultante. No existe sintaxis especial para declarar un constructor: cualquier función puede usarse con `new`; la convención PascalCase es una señal de intención, no una restricción del lenguaje.

```js
function Persona(nombre, edad) {
  this.nombre = nombre;
  this.edad = edad;
}
Persona.prototype.saludar = function() {
  return `Hola, soy ${this.nombre}`;
};

const p = new Persona("Ana", 30);
p.saludar(); // "Hola, soy Ana"
Object.getPrototypeOf(p) === Persona.prototype; // true
```

## Los 4 pasos de `new F(...args)`

```js
// Lo que hace el motor al ejecutar new F(arg1, arg2):

// 1. Crear un objeto vacío con [[Prototype]] = F.prototype
const obj = Object.create(F.prototype);

// 2. Llamar a F con this = obj
const resultado = F.call(obj, arg1, arg2);

// 3. Si F retorna un objeto, ese objeto es el resultado;
//    si retorna primitivo o undefined, el resultado es obj
const instancia = (typeof resultado === "object" && resultado !== null)
  ? resultado
  : obj;

// 4. Retornar instancia
```

```js
// Demostración del paso 3 (retorno de objeto desde el constructor):
function Raro() {
  this.x = 1;
  return { y: 2 }; // retorna objeto → this se descarta
}
const r = new Raro();
r.x; // undefined
r.y; // 2

function Normal() {
  this.x = 1;
  return 42; // retorna primitivo → se ignora, this es el resultado
}
const n = new Normal();
n.x; // 1
```

## `F.prototype` vs `[[Prototype]]` de `F`

Son dos cosas distintas:

- `F.prototype` — objeto que será el `[[Prototype]]` de las instancias creadas con `new F()`.
- `Object.getPrototypeOf(F)` — el `[[Prototype]]` de la función en sí (apunta a `Function.prototype`).

```js
function Animal() {}
// F.prototype es el prototipo de las INSTANCIAS
Animal.prototype; // { constructor: Animal }

// El [[Prototype]] de Animal es Function.prototype
Object.getPrototypeOf(Animal) === Function.prototype; // true
```

## La propiedad `constructor`

Por defecto, `F.prototype.constructor === F`. Permite conocer qué función creó un objeto.

```js
const p = new Persona("Ana", 30);
p.constructor === Persona; // true — encontrado en Persona.prototype
```

> [!warning]
> Si se **reemplaza** `F.prototype` con un objeto nuevo, `constructor` queda apuntando al objeto de reemplazo (normalmente `Object`), no a `F`. Siempre restaurar `constructor` manualmente.
>
> ```js
> function Perro() {}
> Perro.prototype = {
>   ladrar() { return "Guau"; }
>   // ← constructor quedó como Object, no Perro
> };
> Perro.prototype.constructor = Perro; // restaurar
>
> const d = new Perro();
> d.constructor === Perro; // true — ahora correcto
> ```

## Métodos en el prototipo vs métodos en `this`

```js
// Método en el prototipo — una sola función compartida por todas las instancias
function Usuario(nombre) {
  this.nombre = nombre; // propiedad de instancia — una copia por objeto
}
Usuario.prototype.saludar = function() { // una sola función
  return `Hola, ${this.nombre}`;
};

const u1 = new Usuario("Ana");
const u2 = new Usuario("Bob");
u1.saludar === u2.saludar; // true — misma función

// Método en this — una copia de la función por instancia
function UsuarioMal(nombre) {
  this.nombre = nombre;
  this.saludar = function() { return `Hola, ${this.nombre}`; }; // nueva función cada vez
}
const m1 = new UsuarioMal("Ana");
const m2 = new UsuarioMal("Bob");
m1.saludar === m2.saludar; // false — dos funciones distintas en memoria
```

Definir métodos en `this` dentro del constructor crea una nueva función por instancia: mayor consumo de memoria sin beneficio en la mayoría de casos. La excepción es cuando se necesita acceso a variables locales del constructor (closure sobre estado privado).

## `instanceof`

`obj instanceof F` es `true` si `F.prototype` aparece en algún lugar de la cadena de `[[Prototype]]` de `obj`.

```js
const p = new Persona("Ana", 30);
p instanceof Persona; // true — Persona.prototype está en la cadena de p
p instanceof Object;  // true — Object.prototype también está
p instanceof Array;   // false
```

> [!warning]
> Si se reemplaza `F.prototype` después de crear instancias, `instanceof` deja de funcionar correctamente para las instancias ya creadas, porque ellas apuntan al prototipo viejo.

## Función constructora completa — patrón canónico

```js
function Vehiculo(marca, modelo, año) {
  // Propiedades de instancia (una copia por objeto)
  this.marca = marca;
  this.modelo = modelo;
  this.año = año;
  this._km = 0; // convención: privado por acuerdo
}

// Métodos compartidos en el prototipo
Vehiculo.prototype.conducir = function(km) {
  this._km += km;
  return this;
};
Vehiculo.prototype.toString = function() {
  return `${this.marca} ${this.modelo} (${this.año}) — ${this._km} km`;
};

// Propiedad de clase (en la función, no en el prototipo)
Vehiculo.fabricante = "Genérico S.A.";

const v = new Vehiculo("Toyota", "Corolla", 2022);
v.conducir(100).conducir(50);
v.toString(); // "Toyota Corolla (2022) — 150 km"
```

## Cómo funciona por dentro

El motor implementa `new F()` exactamente con los 4 pasos descritos. En V8, la creación del objeto en el paso 1 establece la hidden class inicial del objeto basándose en `F.prototype`. Las propiedades asignadas en `this.x = ...` dentro del constructor van añadiéndose a esa hidden class. Si todas las instancias de `F` asignan las mismas propiedades en el mismo orden, V8 las representa con la misma hidden class y puede optimizar los accesos con inline caches eficientes.

> [!tip]
> Asignar todas las propiedades de instancia dentro del constructor (no condicional ni tardíamente) permite a V8 establecer la forma definitiva del objeto desde el principio y optimizarlo agresivamente. Añadir propiedades después de la construcción introduce transiciones de shape que reducen la eficiencia.

## Notas relacionadas

- [[01 Cadena de Prototipos]]
- [[04 Object.create()]]
- [[06 Herencia Prototípica]]
- [[04 this/04 this en Constructor | this en Constructor]]
- [[03 Clases/09 Clases como Azúcar Sintáctico | Clases como Azúcar Sintáctico]]
