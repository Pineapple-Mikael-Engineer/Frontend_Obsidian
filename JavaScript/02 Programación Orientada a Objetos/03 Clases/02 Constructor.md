---
title: Constructor — Inicialización de instancias en clases
aliases:
  - constructor JS
  - new clase
  - constructor method
tags:
  - javascript
  - api/concepto
  - clases
objeto: global
tipo: concepto
draft: false
---

# Constructor

> [!definicion]
> El método especial `constructor` se ejecuta automáticamente al crear una instancia con `new Clase()`. Su propósito es inicializar el objeto recién creado (`this`). Solo puede existir un `constructor` por clase; si se omite, el motor provee uno vacío de forma implícita.

```js
class Producto {
  constructor(nombre, precio) {
    this.nombre = nombre;
    this.precio = precio;
  }
}

const p = new Producto("Teclado", 49.99);
p.nombre; // → "Teclado"
p.precio; // → 49.99
```

## Reglas del constructor

| Regla | Detalle |
|-------|---------|
| Un solo `constructor` por clase | Segundo `constructor` → `SyntaxError` |
| Omitido en clase base | Motor usa `constructor() {}` |
| Omitido en subclase | Motor usa `constructor(...args) { super(...args); }` |
| `return` primitivo | Se ignora; se retorna `this` (la instancia) |
| `return` de objeto | Reemplaza la instancia — `new` devuelve ese objeto |

## Constructor implícito

```js
class Punto {} // sin constructor explícito

// El motor opera como si hubiera:
// constructor() {}

const p = new Punto();
p instanceof Punto; // → true
```

En una subclase sin constructor explícito, el motor delega al padre automáticamente:

```js
class Animal {
  constructor(nombre) { this.nombre = nombre; }
}
class Perro extends Animal {} // sin constructor

const d = new Perro("Rex");
d.nombre; // → "Rex"  — el constructor implícito llamó super("Rex")
```

## `return` en el constructor

El comportamiento al retornar un objeto desde el constructor sorprende con frecuencia:

```js
class Raro {
  constructor() {
    this.original = true;
    return { reemplazado: true }; // objeto → reemplaza la instancia
  }
}
const r = new Raro();
r.reemplazado;          // → true
r.original;             // → undefined
r instanceof Raro;      // → false  (el objeto retornado no tiene el prototype correcto)

class Normal {
  constructor() {
    this.valor = 42;
    return 99; // primitivo → ignorado, se retorna this
  }
}
new Normal().valor; // → 42
```

Este comportamiento se hereda del mecanismo `new` con funciones constructoras; las clases no lo cambian.

## Constructor con validación

Lanzar desde el constructor detiene la creación de la instancia — el objeto no queda parcialmente construido en ninguna variable.

```js
class Temperatura {
  constructor(celsius) {
    if (typeof celsius !== "number" || isNaN(celsius)) {
      throw new TypeError(`celsius debe ser un número, se recibió: ${typeof celsius}`);
    }
    if (celsius < -273.15) {
      throw new RangeError(`${celsius}°C está por debajo del cero absoluto`);
    }
    this.celsius = celsius;
  }

  get fahrenheit() { return this.celsius * 9 / 5 + 32; }
}

new Temperatura(100).fahrenheit; // → 212
new Temperatura(-300);           // RangeError: -300°C está por debajo del cero absoluto
new Temperatura("frio");         // TypeError
```

## Inicialización con opciones (patrón config object)

Útil cuando el constructor recibe muchos parámetros opcionales:

```js
class Grafico {
  constructor({ ancho = 800, alto = 600, fondo = "#fff", titulo = "" } = {}) {
    this.ancho = ancho;
    this.alto = alto;
    this.fondo = fondo;
    this.titulo = titulo;
  }
}

const g1 = new Grafico({ ancho: 1024, titulo: "Ventas" });
const g2 = new Grafico(); // usa todos los defaults
```

## Cómo funciona por dentro

Al evaluar `new Clase(args)`, el motor ejecuta los siguientes pasos (simplificados del spec):

1. Crea un objeto nuevo con `[[Prototype]] = Clase.prototype`.
2. Llama a la función `constructor` con `this` apuntando al nuevo objeto.
3. Si el constructor retorna un objeto, ese objeto es el resultado de `new`. En cualquier otro caso, el resultado es `this`.

Esto es exactamente lo que hace `new` con funciones constructoras ordinarias — las clases no modifican el mecanismo, solo la sintaxis.

```js
// Lo que new Clase() hace conceptualmente:
function _new(Clase, ...args) {
  const instancia = Object.create(Clase.prototype);
  const resultado = Clase.apply(instancia, args);
  return (resultado !== null && typeof resultado === "object") ? resultado : instancia;
}
```

> [!tip] Buenas prácticas
> - Inicializar todas las propiedades de instancia en el constructor (o como class fields) — evita propiedades `undefined` que aparecen tarde.
> - Validar los argumentos al inicio del constructor y lanzar errores descriptivos.
> - Preferir el patrón config object (`{ prop1, prop2 } = {}`) cuando hay más de 3 parámetros.
> - Mantener el constructor liviano: no hacer peticiones de red ni operaciones costosas; usar métodos de inicialización separados o factories estáticos.

> [!warning] Errores comunes
> - Declarar más de un `constructor` en la misma clase — lanza `SyntaxError` inmediato.
> - Olvidar `super()` en el constructor de una subclase antes de usar `this` — lanza `ReferenceError: Must call super constructor in derived class before accessing 'this'`.
> - Retornar un objeto desde el constructor sin intención — reemplaza la instancia silenciosamente; `instanceof` falla y la instancia pierde su cadena de prototipos.

## Notas relacionadas

- [[index | Clases — índice]]
- [[01 Declaración de Clase]]
- [[04 Propiedades de Instancia]]
- [[08 Herencia (extends, super)]]
