---
title: Propiedades de Instancia — Constructor vs Class Fields
aliases:
  - instance properties
  - class fields
  - campos de clase
  - propiedades de instancia JS
tags:
  - javascript
  - api/concepto
  - clases
objeto: global
tipo: concepto
draft: false
---

# Propiedades de Instancia

> [!definicion]
> Las propiedades de instancia son datos que pertenecen a cada objeto individualmente (no al `prototype`). Se inicializan con `this.prop = valor` en el constructor (forma clásica) o con **class fields** (`prop = valor`) declarados en el cuerpo de la clase directamente (ES2022). Cada instancia tiene su propia copia.

```js
class Punto {
  // Class fields (ES2022)
  x = 0;
  y = 0;

  constructor(x, y) {
    this.x = x; // sobrescribe el field
    this.y = y;
  }
}

const p = new Punto(3, 4);
Object.hasOwn(p, "x"); // → true  — propiedad propia de la instancia
Object.hasOwn(p, "y"); // → true
```

## Dos formas de inicializar

| Forma | Sintaxis | Cuándo se inicializa |
|-------|----------|---------------------|
| Constructor clásico | `this.prop = valor` | Cuando se ejecuta esa línea del constructor |
| Class field | `prop = valor` (cuerpo de clase) | Antes del cuerpo del constructor, por instancia |
| Class field con default | `prop = expresion` | Expresión se evalúa en cada `new` |
| Class field como arrow | `metodo = () => { ... }` | Por instancia, captura `this` del constructor |

## Class fields — Valores por defecto y expresiones

```js
class Conexion {
  host     = "localhost";
  puerto   = 3306;
  intentos = 0;
  creadoEn = new Date(); // se evalúa en cada new, no una vez

  constructor(host, puerto) {
    if (host)   this.host   = host;
    if (puerto) this.puerto = puerto;
  }
}

const c1 = new Conexion();
const c2 = new Conexion("db.prod.com", 5432);

c1.host;     // → "localhost"
c2.host;     // → "db.prod.com"
c1.creadoEn === c2.creadoEn; // → false — cada instancia tiene su propia Date
```

## Class fields como arrow functions — binding correcto de `this`

El mayor beneficio práctico de los class fields: las arrow functions capturan `this` léxicamente desde el contexto del constructor, por lo que el binding siempre apunta a la instancia, incluso al extraer el método.

```js
class Boton {
  constructor(etiqueta) {
    this.etiqueta = etiqueta;
    this.clics = 0;
  }

  // Método en prototype — this se pierde si se extrae
  manejarPrototype() {
    this.clics++;
  }

  // Field con arrow — this siempre es la instancia
  manejar = () => {
    this.clics++;
    console.log(`${this.etiqueta}: ${this.clics} clic(s)`);
  };
}

const btn = new Boton("Enviar");
document.addEventListener("click", btn.manejar);     // OK — this = btn
document.addEventListener("click", btn.manejarPrototype); // ERROR — this = undefined
```

## Diferencia clave: prototype vs field con arrow

```js
class Demo {
  metodoPrototype() { return "prototype"; }
  metodoField = function() { return "field"; };
  arrowField   = () => "arrow";
}

const a = new Demo(), b = new Demo();

// Método en prototype → compartido
a.metodoPrototype === b.metodoPrototype; // → true (misma referencia)

// Field con función → copia por instancia
a.metodoField === b.metodoField;         // → false (funciones distintas)
a.arrowField  === b.arrowField;          // → false (ídem)

// Uso de memoria:
// prototype   → 1 función para N instancias
// field arrow → N funciones para N instancias
```

Preferir métodos en `prototype` para lógica general; class fields con arrow solo cuando se necesite binding garantizado (callbacks de evento, paso a funciones de orden superior).

## Cuándo usar field vs constructor

```js
class Formulario {
  // Field: valor independiente del constructor — default o derivado de constante
  estado = "idle";
  errores = [];

  // Constructor: inicializado con argumentos de entrada
  constructor(endpoint, opciones = {}) {
    this.endpoint = endpoint;
    this.timeout  = opciones.timeout ?? 5000;
    this.metodo   = opciones.metodo  ?? "POST";
  }
}
```

Regla práctica: si el valor no depende de los argumentos de `new`, es candidato a class field. Si depende de los argumentos, va en el constructor.

## Cómo funciona por dentro

Los class fields se implementan como llamadas a `Object.defineProperty` ejecutadas al **inicio de cada llamada al constructor**, antes de que el cuerpo del constructor corra. El orden efectivo es:

1. El motor crea el objeto con `[[Prototype]] = Clase.prototype`.
2. Por cada field declarado en el cuerpo de clase, en orden de aparición, llama `Object.defineProperty(this, "campo", { value: <init>, writable: true, enumerable: true, configurable: true })`.
3. El cuerpo del constructor corre.

Por eso `this.x = x` en el constructor sobrescribe `x = 0` del field: el field ya inicializó la propiedad, y luego el constructor la reasigna.

```js
class Trazado {
  valor = console.log("field inicializado") || 0;

  constructor() {
    console.log("constructor corriendo");
  }
}

new Trazado();
// → "field inicializado"
// → "constructor corriendo"
```

> [!tip] Buenas prácticas
> - Declarar todos los campos de la clase explícitamente como fields aunque se inicialicen en el constructor — mejora la legibilidad y permite que herramientas de análisis estático detecten la forma del objeto.
> - Usar arrow field solo cuando el método se pasará como callback sin control del sitio de llamada.
> - Para lógica compartida entre instancias, el `prototype` es siempre más eficiente en memoria.

> [!warning] Errores comunes
> - Asumir que `errores = []` en el field crea un array compartido entre instancias (como pasaría con `Clase.prototype.errores = []`). Los class fields crean una copia por instancia — el array compartido en prototype es el error clásico de las funciones constructoras manuales.
> - Usar arrow fields para todos los métodos por costumbre: desperdicia memoria y hace que `super.metodo()` no funcione correctamente desde subclases (el field en la instancia sombrea el método del prototype del padre).

## Notas relacionadas

- [[index | Clases — índice]]
- [[02 Constructor]]
- [[03 Métodos de Instancia]]
- [[06 Campos Privados (#)]]
