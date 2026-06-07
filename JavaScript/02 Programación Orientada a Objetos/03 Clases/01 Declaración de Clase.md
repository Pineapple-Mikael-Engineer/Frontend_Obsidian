---
title: Declaración de Clase — Sintaxis, TDZ y modo estricto
aliases:
  - class declaration
  - class expression
  - declaración de clase JS
tags:
  - javascript
  - api/concepto
  - clases
objeto: global
tipo: concepto
draft: false
---

# Declaración de Clase

> [!definicion]
> Una clase se define con la palabra clave `class`. Existen dos formas: **declaración de clase** (`class Nombre { }`) y **expresión de clase** (`const Nombre = class { }`). Ambas crean una función constructora vinculada a un objeto `.prototype`, pero difieren en hoisting y en visibilidad del nombre.

```js
// Declaración de clase
class Rectangulo {
  constructor(ancho, alto) {
    this.ancho = ancho;
    this.alto = alto;
  }
  area() { return this.ancho * this.alto; }
}

// Expresión de clase (anónima)
const Circulo = class {
  constructor(radio) { this.radio = radio; }
  area() { return Math.PI * this.radio ** 2; }
};

typeof Rectangulo; // → "function"
typeof Circulo;    // → "function"
```

## Declaración vs Expresión

| Característica | Declaración `class Foo {}` | Expresión `const Foo = class {}` |
|----------------|---------------------------|----------------------------------|
| Hoisting | TDZ — no accesible antes | TDZ — no accesible antes |
| Nombre en el scope | Sí, en el scope actual | Solo si es expresión nombrada |
| Uso típico | Clase de módulo, exported | Factory, mixin dinámico, inline |
| `Foo.name` | `"Foo"` | `"Foo"` (inferido de la variable) |

## TDZ — las clases no hoistean

A diferencia de las funciones constructoras (que se hoistean completamente), las clases siguen la misma regla que `let`/`const`: existen en la **Temporal Dead Zone** desde el inicio del bloque hasta la declaración.

```js
// Funciones constructoras — hoisting completo
const r = new Rectangulo(2, 3); // OK — Rectangulo fue declarado abajo
function Rectangulo(a, b) { this.a = a; this.b = b; }

// Clases — TDZ
const p = new Punto(1, 2); // ReferenceError: Cannot access 'Punto' before initialization
class Punto { constructor(x, y) { this.x = x; this.y = y; } }
```

## Expresión de clase nombrada

El nombre de la expresión solo es accesible dentro del cuerpo de la clase (p. ej., para recursión), no en el scope externo.

```js
const Fibonacci = class Fib {
  calcular(n) {
    if (n <= 1) return n;
    return new Fib().calcular(n - 1) + new Fib().calcular(n - 2); // "Fib" visible aquí
  }
};

new Fibonacci().calcular(6); // → 8
typeof Fib;                  // ReferenceError: Fib is not defined
```

## Modo estricto automático

El cuerpo de una clase siempre se ejecuta en **strict mode**, independientemente de si el archivo o el scope externo lo declara. Esto afecta principalmente:

- `this` no se coacciona a `undefined` → `window` en funciones sueltas; queda `undefined`.
- Errores silenciosos del modo laxo (asignar a propiedades de solo lectura, duplicar parámetros) lanzan `TypeError` o `SyntaxError`.
- `with` está prohibido dentro del cuerpo.

```js
class Demo {
  mostrar() {
    "use strict" ya está activo aquí aunque no lo declares
    console.log(this); // undefined si se llama sin receptor
  }
}
const fn = new Demo().mostrar;
fn(); // → undefined (strict mode), no window
```

## `typeof Clase === "function"`

Las clases son funciones a todos los efectos del lenguaje. Solo la sintaxis `new` es obligatoria para invocarlas como constructoras.

```js
class Punto {}
typeof Punto;            // → "function"
Punto instanceof Function; // → true
Punto.prototype.constructor === Punto; // → true

Punto(); // TypeError: Class constructor Punto cannot be invoked without 'new'
```

## Cuándo usar declaración vs expresión

- **Declaración** — clase de módulo con nombre estable, cuando se va a `export`. Es la forma estándar.
- **Expresión anónima** — clase inline como valor de retorno de una factory o como argumento a una función de mixin.
- **Expresión nombrada** — cuando la clase necesita referenciarse a sí misma internamente (recursión, `instanceof` interno) sin exponer el nombre al scope exterior.

```js
// Factory con expresión anónima
function crearModelo(schema) {
  return class {
    constructor(datos) { this.datos = datos; }
    validar() { return schema.every(k => k in this.datos); }
  };
}
const Usuario = crearModelo(["nombre", "email"]);
new Usuario({ nombre: "Ana", email: "a@b.com" }).validar(); // → true
```

## Cómo funciona por dentro

Al evaluar una declaración `class Nombre { ... }`, el motor realiza los siguientes pasos conceptuales:

1. Crea una nueva función (el futuro constructor) — esta función es `Nombre`.
2. Crea un objeto nuevo para `Nombre.prototype`.
3. Establece `Nombre.prototype.constructor = Nombre`.
4. Por cada método del cuerpo, añade la propiedad a `Nombre.prototype` usando `Object.defineProperty` con `{ enumerable: false, configurable: true, writable: true }`.
5. Vincula `Nombre` al scope mediante un binding inmutable (similar a `const`).

No hay ningún tipo especial "clase" en el heap de V8 o SpiderMonkey: es una función común con un `.prototype` bien configurado.

> [!tip] Buenas prácticas
> - Declarar clases en la parte superior del módulo, igual que `import`.
> - Usar expresión de clase solo cuando la clase es un valor computado o efímero (mixin, factory).
> - No usar `class` con `var` — perderías el beneficio del TDZ que alerta de dependencias circulares.

> [!warning] Errores comunes
> - Invocar la clase antes de su declaración (TDZ) asumiendo que hoistea como `function`. No hoistea.
> - Intentar llamar a la clase sin `new` — lanza `TypeError` aunque `typeof Clase === "function"`.
> - Asumir que el strict mode solo rige si está en el archivo — el cuerpo de clase lo activa siempre.

## Notas relacionadas

- [[index | Clases — índice]]
- [[02 Constructor]]
- [[09 Clases como Azúcar Sintáctico]]
