---
title: Encapsulación y Abstracción en JavaScript
aliases:
  - encapsulacion JS
  - datos privados JS
tags:
  - javascript
  - api/concepto
  - encapsulacion
draft: false
---

# Encapsulación y Abstracción

> [!definicion]
> Encapsulación es el principio de ocultar el estado interno de un objeto y exponer solo una interfaz pública controlada. En JavaScript existen tres mecanismos para lograrlo: closures (el modelo clásico, pre-ES2022), el Module Pattern (IIFE + closures, para módulos singleton) y los campos privados `#` (integrados en el motor desde ES2022). Esta subsección cubre los dos enfoques basados en closures.

La idea unificadora: las variables que el motor crea en el ámbito léxico de una función son inaccesibles desde fuera de esa función. Un closure — una función interna que referencia esas variables — es la puerta de acceso controlado. Exponer selectivamente closures como métodos de un objeto público equivale a definir una interfaz.

```js
function crearCuenta(saldoInicial) {
  let saldo = saldoInicial;   // inaccesible desde fuera
  return {
    depositar(n) { saldo += n; },
    retirar(n)   { if (n <= saldo) saldo -= n; else throw new Error("Saldo insuficiente"); },
    consultar()  { return saldo; }
  };
}
const cuenta = crearCuenta(100);
cuenta.depositar(50);
cuenta.consultar(); // 150
// cuenta.saldo → undefined — el estado es verdaderamente privado
```

## Mecanismos cubiertos en esta subsección

- [[01 Closures para Datos Privados]] — función factory que devuelve un objeto con métodos; cada llamada produce un closure independiente con su propio estado privado
- [[02 Module Pattern (IIFE + closures)]] — IIFE que ejecuta una sola vez y devuelve la API pública; estado compartido entre todos los consumidores del módulo; Revealing Module Pattern
- [[03 Getters y Setters para Control de Acceso]] — propiedades de acceso combinadas con closures para validación, computación lazy y propiedades de solo lectura

## Campos privados `#` (delegado)

Los campos `#` son la solución ES2022 para encapsulación en clases. El motor enforcea el acceso (no es convención): intentar acceder a `instancia.#campo` desde fuera de la clase lanza `SyntaxError`. Se cubren en [[03 Clases/06 Campos Privados (#) | Campos Privados (#)]].

## Comparación de mecanismos

| Mecanismo | Instancias múltiples | Singleton | Necesita `class` | Soporte nativo del motor |
|---|:---:|:---:|:---:|:---:|
| Closure (factory function) | Sí | No | No | No (convención léxica) |
| Module Pattern (IIFE) | No | Sí | No | No (convención léxica) |
| Campos privados `#` | Sí | No | Sí | Sí (enforced) |

## Notas relacionadas

- [[03 Clases/06 Campos Privados (#) | Campos Privados (#)]] — alternativa ES2022 con soporte del motor
- [[02 Prototipos/index | Prototipos]] — por qué los métodos de closures no se comparten en prototype
- [[04 this/index | this]] — el contexto de `this` dentro de los métodos del objeto devuelto
