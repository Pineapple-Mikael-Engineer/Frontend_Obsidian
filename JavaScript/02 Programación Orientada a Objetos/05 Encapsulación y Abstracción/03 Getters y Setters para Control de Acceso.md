---
title: Getters y Setters para Control de Acceso
aliases:
  - getter setter control acceso
  - accessor properties closure
  - propiedades de acceso JS
tags:
  - javascript
  - api/concepto
  - encapsulacion
draft: false
---

# Getters y Setters para Control de Acceso

> [!definicion]
> Un getter es una función asociada a una propiedad que se invoca al leer (`obj.prop`); un setter, al escribir (`obj.prop = val`). Juntos forman una **propiedad de acceso** (accessor property) en contraposición a una propiedad de datos (data property). La combinación con closures permite que el getter/setter acceda a una variable privada — sin exponerla como propiedad pública — con validación, transformación o lógica de logging en cada lectura o escritura.

```js
function crearPersona(nombreInicial) {
  let _nombre = nombreInicial;

  return {
    get nombre()    { return _nombre; },
    set nombre(val) {
      if (typeof val !== "string" || val.trim() === "") {
        throw new TypeError("Nombre debe ser un string no vacío");
      }
      _nombre = val.trim();
    }
  };
}

const p = crearPersona("  Ana  ");
p.nombre;           // "  Ana  "  (lectura: invoca get)
p.nombre = "Luis";  // escritura: invoca set, valida y almacena "Luis"
p.nombre = "";      // TypeError
p._nombre;          // undefined — la variable privada es inaccesible
```

## Getter sin setter — propiedad de solo lectura

Definir solo `get` sin `set` crea una propiedad que no puede asignarse desde el exterior. En modo estricto, intentar asignar lanza `TypeError`; en modo no estricto, la asignación falla silenciosamente.

```js
function crearCirculo(radio) {
  return {
    get radio()       { return radio; },
    get area()        { return Math.PI * radio ** 2; },
    get perimetro()   { return 2 * Math.PI * radio; }
  };
}

const c = crearCirculo(5);
c.area;       // 78.539...
c.radio = 10; // TypeError en strict mode — no hay setter
```

`area` y `perimetro` son propiedades computadas de solo lectura: no almacenan un valor, lo calculan en cada acceso.

## Lazy initialization con getter

La primera vez que se accede a la propiedad, el getter calcula el valor y lo reemplaza por una propiedad de datos estática con `Object.defineProperty`. Las accesiones posteriores leen directamente el valor cacheado sin invocar el getter.

```js
const configuracion = {
  get datosHeavy() {
    const resultado = JSON.parse(localStorage.getItem("heavy") ?? "{}");
    Object.defineProperty(this, "datosHeavy", {
      value: resultado,
      writable: false,
      configurable: false
    });
    return resultado;
  }
};

// Primera lectura: ejecuta el getter y cachea
configuracion.datosHeavy;
// Lecturas posteriores: propiedad de datos estática — O(1) sin cálculo
configuracion.datosHeavy;
```

El patrón funciona porque `Object.defineProperty` sobre una propiedad configurable (como las definidas con `get`/`set` en literales) puede redefinirla como propiedad de datos. Después de la primera invocación, el getter ya no existe en el objeto.

## Recetas de validación en setter

### Rango numérico

```js
function crearTemperatura(inicial) {
  let _valor = inicial;
  return {
    get celsius()    { return _valor; },
    set celsius(val) {
      if (val < -273.15) throw new RangeError("Por debajo del cero absoluto");
      _valor = val;
    },
    get fahrenheit() { return _valor * 9/5 + 32; }
  };
}
```

### Longitud de string y transformación

```js
function crearEtiqueta(texto) {
  let _texto = texto.slice(0, 50);
  return {
    get texto()    { return _texto; },
    set texto(val) {
      if (typeof val !== "string") throw new TypeError("Debe ser string");
      _texto = val.slice(0, 50);  // trunca silenciosamente al límite
    }
  };
}
```

### Logging de acceso

```js
function crearConAuditoria(valorInicial) {
  let _val = valorInicial;
  const _log = [];

  return {
    get valor() {
      _log.push({ tipo: "lectura", ts: Date.now() });
      return _val;
    },
    set valor(v) {
      _log.push({ tipo: "escritura", anterior: _val, nuevo: v, ts: Date.now() });
      _val = v;
    },
    auditoria() { return [..._log]; }
  };
}
```

## Getters y setters en clases

La misma sintaxis `get`/`set` funciona dentro del cuerpo de una clase. A diferencia de los closures, el getter/setter en clase accede al estado a través de `this` (propiedades públicas o campos `#` privados).

```js
class Rectangulo {
  #ancho;
  #alto;

  constructor(ancho, alto) { this.#ancho = ancho; this.#alto = alto; }

  get area()     { return this.#ancho * this.#alto; }
  get perimetro(){ return 2 * (this.#ancho + this.#alto); }

  set ancho(val) {
    if (val <= 0) throw new RangeError("Ancho debe ser positivo");
    this.#ancho = val;
  }
  get ancho()    { return this.#ancho; }
}
```

En clase, los getters/setters se almacenan en `Clase.prototype` (son compartidos entre instancias, no duplicados). En objetos literales con closures, se almacenan directamente en el objeto (una copia por instancia). Ver [[03 Clases/07 Getters y Setters | Getters y Setters en Clases]] para el detalle de la clase.

## Diferencia con `Object.defineProperty`

La sintaxis `get`/`set` en literales es azúcar equivalente a llamar `Object.defineProperty` con un descriptor de acceso. La diferencia es que `Object.defineProperty` permite controlar `enumerable` y `configurable`.

```js
// Equivalentes:
const obj1 = { get x() { return 42; } };

const obj2 = {};
Object.defineProperty(obj2, "x", {
  get() { return 42; },
  enumerable: true,
  configurable: true
});
```

Un getter definido con literal es `enumerable: true` y `configurable: true` por defecto. Uno definido con `Object.defineProperty` toma los valores que se especifiquen (por defecto `false` para ambos si se omiten).

## Cómo funciona por dentro

El motor almacena las funciones `get` y `set` en el **descriptor de propiedad** del slot interno `[[OwnPropertyDescriptors]]`. Al ejecutar `obj.prop` (operación interna `[[Get]]`), el motor detecta que el descriptor no tiene `value` sino `get` — invoca `get()` con `this = obj` y devuelve el resultado. Al ejecutar `obj.prop = val` (`[[Set]]`), invoca `set(val)` con `this = obj`. No existe slot `value` en una propiedad de acceso — es mutuamente excluyente con `get`/`set` en el descriptor.

Un descriptor de acceso tiene exactamente cuatro campos: `get`, `set`, `enumerable`, `configurable`. No tiene `value` ni `writable` — intentar definir un descriptor con `get` y `value` simultáneamente lanza `TypeError`.

> [!tip] Buenas prácticas
> - Usar getters para propiedades derivadas que se calculan a partir de otras — evitan almacenar estado redundante y garantizan consistencia.
> - Un getter sin setter comunica claramente que la propiedad es de solo lectura. Es más explícito que una convención de prefijo `_`.
> - Para propiedades costosas de calcular que no cambian, aplicar lazy initialization con `Object.defineProperty` dentro del getter.
> - Si la validación del setter es compleja, extraerla a una función privada con nombre descriptivo.

> [!warning] Errores comunes
> - **Recursión infinita:** dentro de un setter o getter, acceder a `this.mismaPropiedad` cuando esa propiedad tiene getter/setter provoca recursión infinita. Usar una variable de respaldo (closure o campo `#`).
>   ```js
>   // Incorrecto:
>   const obj = {
>     get x() { return this.x; }  // llama al getter de nuevo → stack overflow
>   };
>   // Correcto:
>   function crearObj() {
>     let _x = 0;
>     return { get x() { return _x; }, set x(v) { _x = v; } };
>   }
>   ```
> - **Getter con efecto secundario costoso:** si el getter realiza I/O o cálculos pesados, cada lectura de `obj.prop` los ejecuta. Aplicar caching explícito o lazy init.
> - **Setter silencioso en no-strict:** asignar a una propiedad sin setter en modo no estricto falla sin error. Usar `"use strict"` o clases (que siempre son strict) para detectarlo inmediatamente.

## Notas relacionadas

- [[01 Closures para Datos Privados]] — el ámbito léxico que el getter/setter cierra sobre
- [[02 Module Pattern (IIFE + closures)]] — uso de `get valor()` dentro del objeto público de un módulo
- [[01 Objetos Literales/06 Getters y Setters | Getters y Setters en Objetos Literales]] — la misma mecánica sin closures
- [[03 Clases/07 Getters y Setters | Getters y Setters en Clases]] — equivalente en cuerpo de clase con campos `#`
- [[05 Encapsulación y Abstracción/index | Encapsulación]] — visión general de la subsección
