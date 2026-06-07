---
title: this en Función — Default binding
aliases:
  - this en función
  - default binding
  - this en llamada directa
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

# `this` en Función — Default binding

> [!definicion]
> Cuando una función se llama directamente sin receptor explícito (`fn()`, no `obj.fn()`), se aplica el **default binding**: en modo no estricto `this` es el objeto global; en modo estricto `this` es `undefined`. El modo estricto del **cuerpo de la función** es el que determina el resultado, no el del código llamador.

```js
function sinEstricto() {
  console.log(this);
}

function conEstricto() {
  "use strict";
  console.log(this);
}

sinEstricto();   // → window (en browser) | global (en Node CJS)
conEstricto();   // → undefined
```

## Modo estricto: del cuerpo de la función, no del llamador

```js
"use strict"; // modo estricto en el llamador

function noEstricta() {
  // cuerpo sin "use strict"
  console.log(this); // → window  — gana el modo del CUERPO de la función
}

noEstricta();
```

```js
function estricta() {
  "use strict";
  console.log(this); // → undefined — aunque el llamador sea no-estricto
}

estricta();
```

## Callbacks y pérdida de binding

Los callbacks pasados a funciones de orden superior o a temporizadores se invocan sin receptor: aplica default binding.

```js
const obj = {
  valor: 42,
  obtener() {
    return this.valor;
  }
};

// Sin "use strict" en obtener:
const ref = obj.obtener;
ref(); // → undefined (window.valor no existe) — no es 42

setTimeout(obj.obtener, 100); // → undefined — mismo problema
```

El binding se pierde en cuanto la función se separa de su objeto receptor. Esto se trata en detalle en [[06 Pérdida de this]].

## Contaminación accidental del global (modo no estricto)

```js
function inicializar() {
  this.contador = 0;    // crea window.contador = 0 en modo no estricto
  this.nombre = "app";  // crea window.nombre = "app"
}

inicializar(); // llama sin new — this = window

console.log(window.contador); // → 0  (contaminación accidental)
```

En modo estricto la misma llamada lanzaría `TypeError: Cannot set properties of undefined`.

## Cómo funciona por dentro

> [!info]
> Cuando el motor evalúa `fn()`, construye una **Reference** sin base de objeto (base = `undefined`). Al resolver el `ThisValue` del nuevo frame de ejecución, si la Reference no tiene base de objeto se aplica el "Default Binding Rule": si el código del cuerpo de `fn` está en modo estricto, `ThisValue = undefined`; si no, `ThisValue = GlobalObject`. El modo del código **llamante** no interviene en esta decisión.

## Buenas prácticas

> [!tip]
> Activar `"use strict"` (o usar módulos ES, que son estrictamente estrictos) hace que el default binding produzca `undefined` en lugar del objeto global, convirtiendo bugs silenciosos en `TypeError` inmediatos y detectables. Cualquier función que necesite `this` y no sea método o constructora es señal de diseño cuestionable.

## Errores comunes

> [!warning]
> **Extraer un método y llamarlo como función** es el caso más frecuente de default binding inesperado. `const fn = obj.metodo; fn()` — `this` ya no es `obj`. También ocurre al pasar `obj.metodo` como argumento a `forEach`, `map`, `setTimeout`, etc. La solución canónica es [[07 call()|call]] / [[09 bind()|bind]] o una arrow function envolvente.

## Notas relacionadas

- [[index|this — Contexto de invocación]] — árbol de decisión y prioridades
- [[06 Pérdida de this]] — detalles del detaching y soluciones
- [[07 call()]] — explicit binding para cambiar `this` al invocar
- [[09 bind()]] — fijar `this` permanentemente en una función
- [[01 Programación Procedimental/01 Sintaxis Básica/04 Modo Estricto (use strict)|Modo Estricto (use strict)]]
