---
title: Encapsulación con Closures — Module Pattern
aliases:
  - Module Pattern con closures
  - encapsulación IIFE
  - privacidad por closure
tags:
  - javascript
  - api/concepto
  - modulos
draft: false
order: 2
---

# Encapsulación con Closures

> [!definicion]
> El Module Pattern combina [[02 Module Pattern (IIFE)/01 IIFE | IIFE]] y closures para crear estado privado con API pública controlada: la IIFE se ejecuta una vez y crea un scope con variables internas; los métodos del objeto retornado forman closures sobre ese scope y mantienen acceso a él incluso después de que la IIFE haya terminado de ejecutarse.

```js
const contador = (function() {
  let _cuenta = 0; // privado — no accesible desde fuera

  return {
    incrementar() { _cuenta++; },
    decrementar() { _cuenta--; },
    valor()       { return _cuenta; },
    reset()       { _cuenta = 0; }
  };
})();

contador.incrementar();
contador.incrementar();
contador.valor();  // 2
contador._cuenta;  // undefined — privado por closure
```

## Cómo funciona por dentro

La IIFE ejecuta su cuerpo y retorna el objeto con los cuatro métodos. Cuando la IIFE termina, `_cuenta` debería quedar fuera de alcance — pero no lo hace, porque los cuatro métodos del objeto retornado **cierran sobre** el scope de la IIFE. El motor de JS mantiene `_cuenta` viva en memoria mientras el objeto `contador` exista.

```
scope de la IIFE: { _cuenta: 0 }
                       ↑
     incrementar ──────┤
     decrementar ──────┤  closures — referencias al mismo _cuenta
     valor       ──────┤
     reset       ──────┘
```

Cada vez que se llama a `incrementar()`, modifica `_cuenta` en el scope capturado. `valor()` lee ese mismo `_cuenta`. No hay forma de acceder a `_cuenta` desde fuera sin pasar por uno de los métodos del objeto.

## Receta: módulo de carrito de compras

```js
const Carrito = (function() {
  let _items = [];
  let _descuento = 0;

  function _calcularTotal() {
    const bruto = _items.reduce((sum, item) => sum + item.precio * item.cantidad, 0);
    return bruto * (1 - _descuento);
  }

  function agregar(item) {
    const existente = _items.find(i => i.id === item.id);
    if (existente) {
      existente.cantidad += item.cantidad;
    } else {
      _items.push({ ...item });
    }
  }

  function eliminar(id) {
    _items = _items.filter(i => i.id !== id);
  }

  function aplicarDescuento(porcentaje) {
    if (porcentaje < 0 || porcentaje > 1) throw new RangeError("Descuento entre 0 y 1");
    _descuento = porcentaje;
  }

  function resumen() {
    return {
      items: [..._items],        // copia defensiva — el caller no muta _items
      total: _calcularTotal(),
      descuento: _descuento
    };
  }

  return { agregar, eliminar, aplicarDescuento, resumen };
})();

Carrito.agregar({ id: 1, nombre: "Teclado", precio: 89.99, cantidad: 1 });
Carrito.aplicarDescuento(0.1);
Carrito.resumen().total; // 80.991
Carrito._items;          // undefined
```

`_calcularTotal` y `_items` son completamente privados. El caller solo interactúa a través de la API pública. La copia defensiva en `resumen()` evita que el caller mute `_items` a través de la referencia devuelta.

## Módulo singleton vs función fábrica

El Module Pattern estándar produce un **singleton**: una sola instancia con su propio estado.

```js
// Singleton — una sola instancia
const ModuloA = (function() {
  let _estado = 0;
  return { incrementar() { _estado++; }, valor() { return _estado; } };
})();

// Fábrica — múltiples instancias independientes
function crearContador(inicial = 0) {
  let _cuenta = inicial;
  return {
    incrementar() { _cuenta++; },
    valor()       { return _cuenta; }
  };
}

const c1 = crearContador(0);
const c2 = crearContador(100);
c1.incrementar();
c1.valor(); // 1
c2.valor(); // 100  — estado independiente
```

La fábrica no es una IIFE: es una función regular que al invocarse crea un nuevo closure con su propio scope. El patrón es el mismo; solo cambia si se ejecuta una vez o muchas.

## Comparación con campos privados de clase

| Mecanismo | Privacidad | Instancias | Sintaxis |
|---|---|---|---|
| Module Pattern (closure) | Closure sobre scope de IIFE | Singleton (o fábrica manual) | ES5 compatible |
| Campo privado de clase (`#campo`) | Sintaxis de clase | `new Clase()` | ES2022 |
| WeakMap (patrón alternativo) | WeakMap externo | Múltiples | Más verboso |

Los campos `#privado` de clase son la alternativa moderna para código con múltiples instancias. El Module Pattern sigue siendo idiomático para singletons (servicios, stores, configuración global) en código sin framework.

> [!tip]
> El prefijo `_` en `_cuenta`, `_items` es una convención — no garantiza nada técnicamente (en este caso la privacidad la da el closure, no el nombre). En la API pública se omite el `_`.

> [!warning]
> Los closures en el Module Pattern consumen memoria mientras el módulo exista: el scope de la IIFE no puede ser recogido por el garbage collector. Para módulos con grandes colecciones de datos, evaluar si un singleton es la estructura correcta o si conviene instancias con ciclo de vida controlado.

## Notas relacionadas

- [[02 Module Pattern (IIFE)/index | Module Pattern (IIFE)]]
- [[02 Module Pattern (IIFE)/01 IIFE | IIFE]]
- [[02 Module Pattern (IIFE)/03 Patrón Revelador | Patrón Revelador]]
