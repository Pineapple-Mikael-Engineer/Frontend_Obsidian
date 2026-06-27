---
title: Getters y Setters — propiedades de acceso en objeto literal
aliases:
  - getters
  - setters
  - accessor properties
  - get set
tags:
  - javascript
  - api/concepto
  - objetos
objeto: Object
tipo: concepto
draft: false
order: 6
---

# Getters y Setters

> [!definicion]
> Un getter (`get`) y un setter (`set`) son funciones asociadas a una propiedad que el motor invoca automáticamente al leerla o asignarla, respectivamente. Forman una **propiedad de acceso** (accessor property): no almacena un valor en su slot, sino dos referencias a funciones. El descriptor contiene `get`, `set`, `enumerable` y `configurable`, en lugar de `value` y `writable`.

```js
const circulo = {
  _radio: 5,

  get radio() { return this._radio; },
  set radio(r) {
    if (r < 0) throw new RangeError("El radio no puede ser negativo");
    this._radio = r;
  },

  get area() {
    return Math.PI * this._radio ** 2; // calculada, sin almacenamiento
  },
};

console.log(circulo.radio);  // → 5       (invoca get radio)
console.log(circulo.area);   // → 78.53…  (invoca get area)
circulo.radio = 10;          // invoca set radio
console.log(circulo.area);   // → 314.15… (recalculada)
circulo.radio = -1;          // → RangeError
```

## Casos de uso fundamentales

### 1. Propiedad computada (calculada sobre la marcha)

```js
const rectangulo = {
  ancho: 4,
  alto: 6,
  get area()      { return this.ancho * this.alto; },
  get perimetro() { return 2 * (this.ancho + this.alto); },
};

console.log(rectangulo.area);      // → 24
console.log(rectangulo.perimetro); // → 20
```

No hay estado adicional: el valor se recalcula en cada lectura.

### 2. Validación en escritura

```js
const formulario = {
  _email: "",

  get email() { return this._email; },
  set email(valor) {
    if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(valor)) {
      throw new TypeError(`"${valor}" no es un email válido`);
    }
    this._email = valor.trim().toLowerCase();
  },
};

formulario.email = "  Ana@MAIL.com  ";
console.log(formulario.email); // → "ana@mail.com"
```

### 3. Propiedad de solo lectura (getter sin setter)

```js
const constantes = {
  get PI()  { return 3.141592653589793; },
  get TAU() { return 2 * this.PI; },
};

console.log(constantes.PI);   // → 3.141592653589793
constantes.PI = 3;            // silenciado en modo no estricto
// "use strict" → TypeError: Cannot set property PI of #<Object> which has only a getter
```

### 4. Logging y depuración de accesos

```js
function conLog(obj, prop) {
  let valor = obj[prop];
  return Object.create(obj, {
    [prop]: {
      get() { console.log(`[GET] ${prop} = ${valor}`); return valor; },
      set(v) { console.log(`[SET] ${prop}: ${valor} → ${v}`); valor = v; },
      enumerable: true,
      configurable: true,
    },
  });
}

const estado = conLog({ contador: 0 }, "contador");
estado.contador;        // logs: [GET] contador = 0
estado.contador = 5;    // logs: [SET] contador: 0 → 5
```

### 5. Sincronizar con almacenamiento externo

```js
const preferencias = {
  get tema() {
    return localStorage.getItem("tema") ?? "claro";
  },
  set tema(valor) {
    localStorage.setItem("tema", valor);
    document.documentElement.dataset.tema = valor;
  },
};
```

## Descriptor de una propiedad de acceso

```js
const obj = {
  _x: 0,
  get x() { return this._x; },
  set x(v) { this._x = v; },
};

console.log(Object.getOwnPropertyDescriptor(obj, "x"));
// → {
//     get: [Function: get x],
//     set: [Function: set x],
//     enumerable: true,
//     configurable: true
//   }
// No hay "value" ni "writable"
```

## Definir getter/setter con `Object.defineProperty`

```js
const sensor = { _temp: 20 };

Object.defineProperty(sensor, "temperatura", {
  get() { return this._temp; },
  set(v) {
    if (v < -273.15) throw new RangeError("Por debajo del cero absoluto");
    this._temp = v;
  },
  enumerable: true,
  configurable: true,
});

sensor.temperatura = 37;
console.log(sensor.temperatura); // → 37
```

## Cómo funciona por dentro

El motor almacena las funciones `get` y `set` en el **descriptor de propiedad** de ese slot. Al ejecutar `obj.prop` (lectura), el algoritmo `[[Get]]` detecta que el descriptor es de tipo acceso y llama a `descriptor.get.call(obj)` — `this` es el objeto receptor. Al ejecutar `obj.prop = v` (escritura), `[[Set]]` llama a `descriptor.set.call(obj, v)`. Ningún valor se almacena directamente en el slot de la propiedad; el almacenamiento es responsabilidad de la función `get`/`set` (típicamente redirigen a una propiedad con prefijo `_`).

Las propiedades de acceso no tienen `value` ni `writable`. Intentar definir `value` y `get` en el mismo descriptor lanza `TypeError: Invalid property descriptor. Cannot both specify accessors and a value or writable attribute`.

> [!tip] Buenas prácticas
> - Nombrar la propiedad de respaldo con prefijo `_` por convención: `_radio`, `_email`, `_valor`.
> - Preferir getters computados sobre métodos de lectura (`get area()` vs `calcularArea()`) cuando la semántica es "leer un atributo", no "ejecutar una acción".
> - Combinar getter/setter con `Object.defineProperty` para marcar la propiedad de respaldo como no-enumerable si no debe aparecer en iteraciones.

> [!warning] Errores comunes
> - Getter recursivo: `get x() { return this.x; }` — referencia infinita; el getter se llama a sí mismo. La propiedad de respaldo debe tener un nombre diferente.
> - Setter sin getter: la propiedad parece existir pero siempre devuelve `undefined` al leerla. En modo estricto, puede causar confusión silenciosa.
> - Getter que tiene efectos secundarios lentos y se llama en loops — el costo se acumula porque se recalcula en cada acceso.
> - Olvidar que las propiedades de acceso no son serializables por `JSON.stringify`: el getter no se llama, la propiedad simplemente no aparece en el JSON.

## Notas relacionadas

- [[01 Creación y Propiedades]] — descriptores de propiedad en detalle
- [[03 Clases/07 Getters y Setters.md | Getters y Setters en Clases]] — misma sintaxis dentro de `class`
- [[05 Encapsulación y Abstracción/03 Getters y Setters para Control de Acceso.md | Getters y Setters para Control de Acceso]] — patrón encapsulación
