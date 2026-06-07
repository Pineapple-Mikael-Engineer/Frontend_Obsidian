---
title: Campos Privados (#) — Encapsulamiento real en clases JS
aliases:
  - private fields
  - campos privados JS
  - hash private
  - "#campo"
tags:
  - javascript
  - api/concepto
  - clases
objeto: global
tipo: concepto
draft: false
---

# Campos Privados (`#`)

> [!definicion]
> Un campo privado se declara con el prefijo `#` en el cuerpo de la clase. Solo es accesible desde dentro del cuerpo de esa clase — el motor lanza `SyntaxError` (en tiempo de análisis) o `TypeError` (en tiempo de ejecución) si se intenta acceder desde fuera. No es una convención como `_campo`: el acceso externo es imposible.

```js
class CuentaBancaria {
  #saldo = 0; // campo privado

  constructor(saldoInicial) {
    if (saldoInicial < 0) throw new RangeError("Saldo inicial no puede ser negativo");
    this.#saldo = saldoInicial;
  }

  depositar(monto) { this.#saldo += monto; }
  retirar(monto) {
    if (monto > this.#saldo) throw new Error("Fondos insuficientes");
    this.#saldo -= monto;
  }

  get saldo() { return this.#saldo; }
}

const cuenta = new CuentaBancaria(100);
cuenta.depositar(50);
cuenta.saldo;    // → 150
cuenta.#saldo;   // SyntaxError: Private field '#saldo' must be declared in an enclosing class
```

## Declaración obligatoria

Los campos privados **deben declararse** en el cuerpo de la clase antes de usarse. No se pueden crear dinámicamente con `this.#nuevoCampo = x` si `#nuevoCampo` no fue declarado.

```js
class A {
  #x; // declaración (undefined por defecto)

  init(val) {
    this.#x = val;      // OK — #x fue declarado
    this.#y = val;      // SyntaxError — #y no fue declarado
  }
}
```

## `#campo` vs `_campo` por convención

| Aspecto | `#campo` (privado real) | `_campo` (convención) |
|---------|------------------------|----------------------|
| Acceso externo | Imposible (motor lo bloquea) | Posible — solo señal visual |
| Detección en debugger | No aparece como propiedad propia | Aparece en `Object.keys`, devtools |
| `in` para verificar | `#campo in obj` funciona | `"_campo" in obj` funciona |
| Herencia | No accesible en subclases | Accesible normalmente |
| Colisión de nombres | Imposible entre clases | Posible si dos clases usan el mismo nombre |

## Métodos privados

```js
class Analizador {
  #texto;

  constructor(texto) { this.#texto = texto; }

  #limpiar(str) {
    return str.trim().toLowerCase().replace(/\s+/g, " ");
  }

  analizar() {
    const limpio = this.#limpiar(this.#texto);
    return limpio.split(" ").length;
  }
}

const a = new Analizador("  Hola   Mundo  ");
a.analizar();  // → 2
a.#limpiar;    // SyntaxError
```

## Campos y métodos estáticos privados

```js
class Sesion {
  static #instancias = 0;
  static #MAX = 10;

  #id;

  constructor() {
    if (Sesion.#instancias >= Sesion.#MAX) {
      throw new Error(`Límite de ${Sesion.#MAX} sesiones alcanzado`);
    }
    Sesion.#instancias++;
    this.#id = Sesion.#instancias;
  }

  static get activas() { return Sesion.#instancias; }
  get id() { return this.#id; }
}

const s1 = new Sesion();
const s2 = new Sesion();
Sesion.activas; // → 2
s1.id;          // → 1
```

## Operador `in` con campos privados

`#campo in objeto` permite verificar si un objeto tiene ese campo privado sin lanzar error. Útil en métodos estáticos o de clase que reciben un objeto de tipo desconocido.

```js
class Punto {
  #x; #y;
  constructor(x, y) { this.#x = x; this.#y = y; }

  static esPunto(obj) {
    return #x in obj && #y in obj;
  }
}

class Circulo {
  #x; #y; #r;
  constructor(x, y, r) { this.#x = x; this.#y = y; this.#r = r; }
}

Punto.esPunto(new Punto(1, 2));   // → true
Punto.esPunto(new Circulo(0, 0, 5)); // → false — Circulo tiene #x/#y propios, pero no los de Punto
```

> [!info] Cada `#campo` es único por clase
> `Punto.#x` y `Circulo.#x` son campos distintos aunque tengan el mismo nombre. El motor los distingue por la clase que los declara. `Punto.esPunto` verifica los `#x`/`#y` de `Punto`, no los de `Circulo`.

## Cómo funciona por dentro

El motor implementa los campos privados con un mecanismo similar a un **WeakMap interno** por clase:

```
WeakMap_Punto_#x: { instanciaA → 3, instanciaB → 7, ... }
WeakMap_Punto_#y: { instanciaA → 4, instanciaB → 2, ... }
```

- La clave es la instancia, el valor es el campo privado.
- Al acceder a `this.#x`, el motor busca la instancia (`this`) como clave en el WeakMap de `#x` de esa clase.
- Si la instancia no es la clave correcta (acceso desde fuera), el lookup falla con `TypeError`.
- Los campos privados no están en el objeto propio (`Object.keys`, `in`, `Reflect.ownKeys`) ni en ningún `[[Prototype]]`.

```js
const cuenta = new CuentaBancaria(100);
Object.keys(cuenta);         // → [] o []  — #saldo no aparece
"#saldo" in cuenta;          // → false
Reflect.ownKeys(cuenta);     // → []  — tampoco
```

> [!tip] Buenas prácticas
> - Declarar todos los campos privados al inicio del cuerpo de la clase, antes de los métodos, para documentar la forma del objeto.
> - Combinar `#campo` con getter público (`get campo()`) para crear propiedades de solo lectura.
> - Usar `static #campo` para estado compartido que no debe ser visible ni modificable desde fuera de la clase (contadores, cachés, configuración interna).

> [!warning] Errores comunes
> - Olvidar declarar el campo y luego intentar usarlo en el constructor — `SyntaxError` inmediato. Los campos privados requieren declaración explícita.
> - Intentar acceder a `#campo` de la clase padre desde la subclase — los campos privados no se heredan en el sentido tradicional; la subclase no puede acceder a los `#campos` del padre.
> - Asumir que la serialización con `JSON.stringify` captura los campos privados — no los captura; se necesita un método `toJSON()` explícito.

## Notas relacionadas

- [[index | Clases — índice]]
- [[04 Propiedades de Instancia]]
- [[05 Métodos y Propiedades Estáticas]]
- [[07 Getters y Setters]]
