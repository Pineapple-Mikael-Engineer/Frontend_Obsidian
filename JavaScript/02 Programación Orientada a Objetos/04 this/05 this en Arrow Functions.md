---
title: this en Arrow Functions — Binding léxico
aliases:
  - this en arrow functions
  - binding léxico
  - this léxico
tags:
  - javascript
  - api/concepto
  - this
objeto: global
tipo: concepto
muta: false
asincrono: false
draft: false
---

# `this` en Arrow Functions — Binding léxico

> [!definicion]
> Las arrow functions **no tienen `this` propio**. Capturan el `this` del ámbito léxico donde fueron definidas en tiempo de creación, no en tiempo de llamada. Ese valor no puede cambiarse con `call`, `apply` ni `bind`.

```js
class Temporizador {
  constructor() {
    this.segundos = 0;
  }

  iniciar() {
    setInterval(() => {
      this.segundos++;            // this = instancia de Temporizador
      console.log(this.segundos); // → 1, 2, 3 …
    }, 1000);
  }
}

const t = new Temporizador();
t.iniciar();
```

Con una función regular en lugar de la arrow, `this` dentro del `setInterval` sería `undefined` (modo estricto) o `window`.

## Función regular vs arrow — diferencias clave

| Característica | Función regular | Arrow function |
|---|---|---|
| `this` propio | Sí — determinado en call-site | No — captura el del cierre |
| `arguments` | Sí — objeto local | No — hereda del cierre |
| `new` | Sí — puede usarse como constructora | No — lanza `TypeError` |
| `super` | Sí (en métodos de clase) | No — hereda del cierre |
| Puede fijar `this` con `call/apply/bind` | Sí | No — argumento ignorado |

## Caso de uso principal: callbacks en métodos de clase

```js
class Notificador {
  constructor(mensaje) {
    this.mensaje = mensaje;
    this.visto = false;
  }

  suscribir(boton) {
    // Arrow captura this = instancia de Notificador en tiempo de definición
    boton.addEventListener("click", () => {
      this.visto = true;
      console.log(this.mensaje);
    });
  }
}
```

## `call`/`apply`/`bind` se ignoran

```js
const flecha = () => console.log(this);

flecha.call({ nombre: "ignorado" });   // → el this léxico, no { nombre: "ignorado" }
flecha.apply(window);                  // → el this léxico
const atada = flecha.bind({ x: 1 });
atada();                               // → el this léxico — bind devuelve la misma arrow
```

`bind` sobre una arrow devuelve una nueva función que también ignora el receptor fijado.

## La trampa: método definido como arrow en un literal de objeto

```js
const config = {
  prefijo: "APP",
  formatear: () => {
    // this NO es config — es el this del scope donde se definió el literal
    // (el módulo / script global)
    return `${this.prefijo}: ...`; // → "undefined: ..." o error
  }
};

config.formatear(); // → "undefined: ..."
```

La arrow captura el `this` del scope que rodea al literal `{}`, que normalmente es el módulo (`undefined`) o `window`. Para métodos de objeto, usar siempre función regular o la shorthand `metodo() {}`.

## `new` con arrow: siempre TypeError

```js
const Fn = () => {};
new Fn(); // → TypeError: Fn is not a constructor
```

Las arrows no tienen `[[Construct]]` interno.

## Cómo funciona por dentro

> [!info]
> En tiempo de parseo, el motor no le asigna un slot `[[ThisValue]]` propio a la arrow function. Al invocarse, en lugar de determinar un nuevo binding mediante el call-site, el motor sube al cierre léxico circundante y toma el `[[ThisValue]]` que ya estaba registrado en ese entorno. Internamente esto se modela como: la arrow function accede a `this` del entorno léxico de su creación exactamente igual que accede a variables externas (`x`, `y`, etc.) mediante el mecanismo de closure normal.

## Buenas prácticas

> [!tip]
> Usar arrows para callbacks y funciones anónimas dentro de métodos de clase — son el antídoto natural a la pérdida de `this`. Evitar arrows para: métodos de objeto literal, métodos de prototipo, funciones que puedan necesitar distintos contextos de invocación, y cualquier función que se quiera usar como constructora.

## Errores comunes

> [!warning]
> **Definir handlers de eventos como arrows en el constructor** y luego intentar removerlos falla porque cada invocación del constructor crea una nueva función flecha (referencia distinta). `removeEventListener` requiere la misma referencia. Guardar la arrow en `this.handler = () => {}` y remover `this.handler` resuelve el problema. La alternativa es usar campos de clase: `handler = () => {}` crea una copia por instancia pero con referencia estable por instancia.

## Notas relacionadas

- [[index|this — Contexto de invocación]] — árbol de decisión
- [[06 Pérdida de this]] — alternativas para preservar `this` (arrow, bind)
- [[03 Programación Funcional/03 Arrow Functions/index|Arrow Functions]] — sintaxis, return implícito y cuándo usarlas
- [[03 Clases/04 Propiedades de Instancia|Propiedades de Instancia]] — campos de clase con arrow como patrón de handler
