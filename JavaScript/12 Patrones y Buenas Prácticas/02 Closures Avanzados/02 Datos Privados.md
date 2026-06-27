---
title: Datos Privados con Closures — Encapsulación sin clases
aliases:
  - datos privados closure
  - encapsulación closure
  - private data closure
tags:
  - javascript
  - api/concepto
  - patrones
draft: false
order: 2
---

# Datos Privados con Closures

> [!definicion]
> Las **closures** son el mecanismo clásico de privacidad en JavaScript: las variables declaradas en el scope de una función fábrica son inaccesibles desde el exterior — no aparecen como propiedades del objeto retornado, no pueden asignarse directamente, y solo son manipulables a través de los métodos que la fábrica expone explícitamente.

```js
function crearCuenta(saldoInicial) {
  let saldo = saldoInicial;  // privado — no accesible como cuenta.saldo
  const historial = [];      // privado

  return {
    depositar(cantidad) {
      if (cantidad <= 0) throw new RangeError('Cantidad debe ser positiva');
      saldo += cantidad;
      historial.push({ tipo: 'deposito', cantidad, saldoResultante: saldo });
    },
    retirar(cantidad) {
      if (cantidad > saldo) throw new RangeError('Saldo insuficiente');
      saldo -= cantidad;
      historial.push({ tipo: 'retiro', cantidad, saldoResultante: saldo });
    },
    getSaldo()    { return saldo; },
    getHistorial() { return [...historial]; }  // copia defensiva
  };
}

const cuenta = crearCuenta(1000);
cuenta.depositar(500);
cuenta.getSaldo();         // 1500
cuenta.saldo;              // undefined — genuinamente privado
cuenta.saldo = -99999;     // silencioso — no afecta al saldo interno
```

## Por qué funciona

Cuando `crearCuenta` retorna, su scope de ejecución no se destruye — el objeto retornado mantiene referencias a los métodos, y esos métodos capturan el scope mediante closure. El motor mantiene vivo ese scope mientras existan referencias a alguno de los métodos. Desde fuera del closure, `saldo` e `historial` son inalcanzables.

## Ventajas sobre la convención `_variable`

El patrón `_saldo` (prefijo por convención) es solo eso: una convención. Nada impide `cuenta._saldo = -1000`. Con closures, la invariante se protege en el runtime — no hay forma de romper el encapsulamiento desde fuera sin modificar la fábrica.

```js
// Convención — no garantiza nada
class CuentaInsegura {
  constructor(saldo) { this._saldo = saldo; }
}
const c = new CuentaInsegura(1000);
c._saldo = -999;  // válido, el objeto lo acepta

// Closure — genuinamente privado
const cuenta = crearCuenta(1000);
cuenta.saldo = -999;  // no afecta a nada
```

## Múltiples instancias independientes

Cada llamada a la fábrica crea un scope nuevo — instancias con estado propio y sin interferencia:

```js
const cuenta1 = crearCuenta(500);
const cuenta2 = crearCuenta(2000);

cuenta1.depositar(100);
cuenta1.getSaldo();  // 600
cuenta2.getSaldo();  // 2000 — no afectado
```

## Copia defensiva en getters

Retornar `[...historial]` en lugar de `historial` evita que el llamador mute el array interno desde fuera:

```js
const hist = cuenta.getHistorial();
hist.push({ tipo: 'trampa' });  // modifica la copia, no el historial interno
cuenta.getHistorial().length;   // sin cambios
```

Sin la copia defensiva, el llamador obtendría una referencia al array real y podría modificarlo directamente.

## Comparación con campos privados de clase (`#`)

Los campos privados de clase (`#campo`) son el mecanismo nativo de ES2022 para privacidad:

| Aspecto | Closure | `#campo` de clase |
|---|---|---|
| Privacidad real | Sí (scope inaccesible) | Sí (sintaxis del lenguaje) |
| Rendimiento | Cada instancia tiene sus propias funciones | Métodos compartidos en prototipo |
| DevTools | Variables no visibles en inspección | Campo marcado como privado, visible en DevTools |
| Funciones privadas | Sí (funciones locales en el scope) | No directamente (`#metodo` es más verboso) |
| `this` | No necesario | Necesario; riesgo de pérdida |
| Herencia | No | Sí, con `extends` |
| Uso adecuado | Módulos, factories, alto aislamiento | Clases con herencia, muchas instancias |

```js
// Equivalente con clase y campos privados
class Cuenta {
  #saldo;
  #historial = [];

  constructor(saldoInicial) { this.#saldo = saldoInicial; }

  depositar(cantidad) {
    if (cantidad <= 0) throw new RangeError('Cantidad debe ser positiva');
    this.#saldo += cantidad;
    this.#historial.push({ tipo: 'deposito', cantidad });
  }

  getSaldo()     { return this.#saldo; }
  getHistorial() { return [...this.#historial]; }
}
```

Para jerarquías o muchas instancias, la clase con `#` es más performante. Para módulos independientes donde no se necesita herencia, la closure es más simple y elimina los problemas de `this`.

> [!tip]
> Cuando se necesite exponer solo lectura de un valor, el getter del closure puede retornar directamente el primitivo (`return saldo`) sin copia — los primitivos son inmutables por valor. La copia defensiva solo es necesaria para objetos y arrays.

> [!warning]
> Las closures no son completamente invisibles: en la mayoría de DevTools, pausando con un breakpoint dentro de un método se puede ver el scope del closure con sus variables. No son un mecanismo de seguridad criptográfica — son encapsulación de diseño, no protección contra código malicioso en el mismo proceso.

## Notas relacionadas

- [[01 Fábricas de Funciones|Fábricas de Funciones]]
- [[index|Closures Avanzados — Índice]]
