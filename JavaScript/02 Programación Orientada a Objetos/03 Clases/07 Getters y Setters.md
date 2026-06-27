---
title: Getters y Setters — Propiedades de acceso en clases JS
aliases:
  - getters setters clase
  - get set JS
  - accessors class
  - propiedades de acceso
tags:
  - javascript
  - api/concepto
  - clases
objeto: global
tipo: concepto
draft: false
order: 7
---

# Getters y Setters

> [!definicion]
> Un getter (`get`) y un setter (`set`) son funciones que se invocan automáticamente al leer o asignar una propiedad, respectivamente. En el cuerpo de una clase se declaran con las palabras clave `get` y `set` antes del nombre; el motor los añade a `Clase.prototype` como propiedades de acceso no enumerables.

```js
class Temperatura {
  #celsius;

  constructor(celsius) { this.#celsius = celsius; }

  get celsius()    { return this.#celsius; }
  set celsius(val) {
    if (val < -273.15) throw new RangeError("Por debajo del cero absoluto");
    this.#celsius = val;
  }

  get fahrenheit() { return this.#celsius * 9 / 5 + 32; }
  get kelvin()     { return this.#celsius + 273.15; }
}

const t = new Temperatura(100);
t.fahrenheit; // → 212    — getter, sin ()
t.celsius = 0;
t.kelvin;     // → 273.15
t.celsius = -300; // RangeError
```

## Getter sin setter — propiedad de solo lectura

Si se declara un `get` sin su `set` correspondiente, intentar asignar a esa propiedad en modo estricto lanza `TypeError`; en modo no estricto, la asignación se ignora silenciosamente.

```js
class Circulo {
  constructor(radio) { this.radio = radio; }
  get area()         { return Math.PI * this.radio ** 2; }
  get perimetro()    { return 2 * Math.PI * this.radio; }
}

const c = new Circulo(5);
c.area;       // → 78.53...
c.area = 100; // TypeError (strict): Cannot set property area of #<Circulo> which has only a getter
```

## Getter que computa vs propiedad almacenada

| Tipo | Cuándo usar |
|------|------------|
| Getter computado | Valor derivado de otras propiedades, no costoso de calcular |
| Propiedad almacenada | Valor independiente o costoso de calcular (cachear manualmente) |
| Getter con caché | Valor costoso que se calcula una sola vez y se guarda en `#campo` privado |

```js
class Poligono {
  #vertices;
  #perimetroCache = null;

  constructor(vertices) { this.#vertices = vertices; }

  get perimetro() {
    if (this.#perimetroCache === null) {
      this.#perimetroCache = this.#vertices.reduce((sum, v, i, arr) => {
        const siguiente = arr[(i + 1) % arr.length];
        return sum + Math.hypot(siguiente[0] - v[0], siguiente[1] - v[1]);
      }, 0);
    }
    return this.#perimetroCache;
  }
}
```

## Setter con validación

El patrón más frecuente: el setter valida la entrada antes de asignar al campo privado.

```js
class Usuario {
  #nombre;
  #edad;

  set nombre(val) {
    if (typeof val !== "string" || val.trim() === "") {
      throw new TypeError("El nombre debe ser un string no vacío");
    }
    this.#nombre = val.trim();
  }
  get nombre() { return this.#nombre; }

  set edad(val) {
    if (!Number.isInteger(val) || val < 0 || val > 150) {
      throw new RangeError(`Edad inválida: ${val}`);
    }
    this.#edad = val;
  }
  get edad() { return this.#edad; }

  constructor(nombre, edad) {
    this.nombre = nombre; // invoca el setter
    this.edad   = edad;
  }
}

const u = new Usuario("Ana", 30);
u.nombre;      // → "Ana"
u.nombre = ""; // TypeError
u.edad = -5;   // RangeError
```

## Getters en subclases

Los getters se heredan a través de `Clase.prototype` igual que los métodos. Una subclase puede sobreescribirlos.

```js
class Figura {
  get tipo() { return "figura"; }
  get descripcion() { return `Soy una ${this.tipo}`; } // usa this.tipo dinámicamente
}

class Cuadrado extends Figura {
  constructor(lado) { super(); this.lado = lado; }
  get tipo() { return "cuadrado"; } // sobreescribe
  get area()  { return this.lado ** 2; }
}

new Cuadrado(4).descripcion; // → "Soy un cuadrado"
new Cuadrado(4).area;        // → 16
```

## Cómo funciona por dentro

El motor añade getters y setters al prototype usando `Object.defineProperty` con descriptor de acceso:

```js
Object.defineProperty(Clase.prototype, "nombrePropiedad", {
  get: function() { /* cuerpo del getter */ },
  set: function(val) { /* cuerpo del setter */ }, // omitido si no hay setter
  enumerable:   false,
  configurable: true,
});
```

El mismo mecanismo opera en objetos literales (`{ get prop() {} }`). La diferencia es que en objetos literales la propiedad es `enumerable: true` por defecto; en cuerpos de clase, `enumerable: false`.

```js
// Verificar el descriptor:
Object.getOwnPropertyDescriptor(Temperatura.prototype, "celsius");
// → { get: [Function: get celsius], set: [Function: set celsius], enumerable: false, configurable: true }
```

> [!tip] Buenas prácticas
> - Combinar `#campo` privado + getter público: patrón estándar para encapsulamiento con acceso de lectura.
> - Usar getters para propiedades derivadas (nunca mantener sincronizadas manualmente varias propiedades que se pueden calcular).
> - Si el getter hace trabajo costoso, cachear el resultado en otro `#campo` privado.
> - En el setter, siempre validar antes de asignar, y lanzar `TypeError` o `RangeError` con mensaje descriptivo.

> [!warning] Errores comunes
> - Declarar un getter con el mismo nombre que un campo de clase — el descriptor de acceso y el campo de datos son mutuamente excluyentes; el campo de clase (data property) sombrea al getter del prototype desde la instancia.
> - Llamar al getter con `()`: `t.celsius()` — lanza `TypeError: t.celsius is not a function`. Los getters se acceden como propiedades, no como métodos.
> - Crear un setter sin getter: la propiedad queda de solo escritura — leer retorna `undefined` sin error.
> - Ciclo infinito: `set valor(v) { this.valor = v; }` — el setter se llama a sí mismo infinitamente. Siempre asignar al campo privado `#campo`, no a la misma propiedad del getter/setter.

## Notas relacionadas

- [[index | Clases — índice]]
- [[06 Campos Privados (#)]]
- [[03 Métodos de Instancia]]
- [[04 Propiedades de Instancia]]
