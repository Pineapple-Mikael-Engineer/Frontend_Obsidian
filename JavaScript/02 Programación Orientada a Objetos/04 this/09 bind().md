---
title: Function.prototype.bind() — Fijar this permanentemente
aliases:
  - bind
  - Function.prototype.bind
  - fn.bind
  - función atada
  - bound function
tags:
  - javascript
  - api/metodo
  - this
objeto: Function
tipo: metodo
retorna: Function
muta: false
asincrono: false
draft: false
order: 9
---

# `Function.prototype.bind()`

> [!definicion]
> `fn.bind(thisArg, arg1, arg2, …)` devuelve una **nueva función** — una *bound function* — con `this` fijado permanentemente a `thisArg`. A diferencia de `call`/`apply`, no invoca `fn`; devuelve una versión atada. Los argumentos adicionales quedan pre-fijados como parciales (partial application).

```js
function saludar(saludo, puntuacion) {
  return `${saludo}, ${this.nombre}${puntuacion}`;
}

const usuario = { nombre: "Ana" };

const saludarAna = saludar.bind(usuario, "Hola");
saludarAna("!");    // → "Hola, Ana!"
saludarAna("...");  // → "Hola, Ana..."
```

## Firma

```
fn.bind(thisArg[, arg1[, arg2[, …]]])
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `thisArg` | any | `this` fijado en la función devuelta |
| `arg1, arg2, …` | any | Argumentos pre-fijados (partial application) |
| **Retorna** | Function | Nueva bound function con `this` y args fijados |

## El `this` fijado no puede cambiarse

Una vez creada, la bound function ignora cualquier intento de cambiar `this`:

```js
const atada = fn.bind({ x: 1 });

atada.call({ x: 99 });           // → this sigue siendo { x: 1 }
atada.apply({ x: 99 }, []);      // → this sigue siendo { x: 1 }
atada.bind({ x: 99 })();         // → this sigue siendo { x: 1 }
```

## Excepción: `new` ignora el `bind`

Si la función atada se usa como constructora con `new`, el motor crea el objeto nuevo normalmente y el `thisArg` fijado se descarta. Esta es la única vía de escape al binding de `bind`.

```js
function Punto(x, y) {
  this.x = x;
  this.y = y;
}

const PuntoX5 = Punto.bind(null, 5); // x pre-fijado a 5
const p = new PuntoX5(10);           // new ignora null como thisArg
console.log(p.x, p.y);               // → 5, 10
console.log(p instanceof Punto);     // → true
```

## Partial application — argumentos pre-fijados

```js
function multiplicar(a, b) { return a * b; }

const doble = multiplicar.bind(null, 2);
const triple = multiplicar.bind(null, 3);

doble(7);   // → 14
triple(7);  // → 21

// Receta: formatear con prefijo fijo
const log = console.log.bind(console, "[APP]");
log("Iniciando...");  // → "[APP] Iniciando..."
log("Listo");         // → "[APP] Listo"
```

## Receta: handlers en el constructor

El patrón estándar pre-campos-de-clase para que todos los handlers de una clase tengan `this` correcto:

```js
class Formulario {
  constructor(endpoint) {
    this.endpoint = endpoint;
    this.datos = {};

    // Atar en el constructor — misma referencia, removible con removeEventListener
    this.handleSubmit = this.handleSubmit.bind(this);
    this.handleChange = this.handleChange.bind(this);
  }

  handleSubmit(e) {
    e.preventDefault();
    fetch(this.endpoint, { method: "POST", body: JSON.stringify(this.datos) });
  }

  handleChange(e) {
    this.datos[e.target.name] = e.target.value;
  }

  montar(form) {
    form.addEventListener("submit", this.handleSubmit);
    form.addEventListener("change", this.handleChange);
  }

  desmontar(form) {
    form.removeEventListener("submit", this.handleSubmit);
    form.removeEventListener("change", this.handleChange);
  }
}
```

## `bind` vs campo de clase con arrow

| Criterio | `bind` en constructor | Campo de clase con arrow |
|---|---|---|
| Sintaxis | `this.m = this.m.bind(this)` | `m = () => { … }` |
| Referencia estable por instancia | Sí | Sí |
| Aparece en la clase como método | Sí, en instancia, no en prototipo | Sí, solo en instancia |
| Uso de `super` dentro | Sí (es función regular en prototype) | No — arrow no tiene `super` |
| Compatible con herencia | Sí | Con limitaciones |

## Cómo funciona por dentro

> [!info]
> `bind` crea una **Bound Function Exotic Object** — un objeto con los slots internos `[[BoundTargetFunction]]`, `[[BoundThis]]` y `[[BoundArguments]]`. Al invocar la bound function, el motor no determina `this` mediante el call-site; en su lugar lee `[[BoundThis]]` directamente. Los `[[BoundArguments]]` se anteponen a los argumentos de la llamada concreta. Cuando se invoca con `new`, el motor detecta que el target es una bound function, ignora `[[BoundThis]]` y aplica new binding sobre `[[BoundTargetFunction]]`. La propiedad `name` de la bound function es `"bound " + fn.name`; `length` es `fn.length - args_prefijados`.

```js
function fn(a, b, c) {}
const atada = fn.bind(null, 1);

atada.name;    // → "bound fn"
atada.length;  // → 2  (3 - 1 arg pre-fijado)
```

## Buenas prácticas

> [!tip]
> En código moderno con soporte de campos de clase (ES2022 / TypeScript / Babel), preferir `handler = () => {}` sobre `bind` en el constructor — más legible y elimina el riesgo de olvidar atar un nuevo método. Reservar `bind` para: partial application, APIs de callbacks donde el array de bindings ya existe y se crea en setup, y para código compatible con entornos sin campos de clase.

## Errores comunes

> [!warning]
> **Usar `bind` inline en `addEventListener`** crea una nueva referencia en cada llamada, imposibilitando `removeEventListener`:
> ```js
> // MAL — cada llamada a montar crea una función diferente
> form.addEventListener("submit", this.handleSubmit.bind(this));
> form.removeEventListener("submit", this.handleSubmit.bind(this)); // no remueve nada
> ```
> La solución es guardar la referencia atada una vez (`this.handleSubmit = this.handleSubmit.bind(this)`) en el constructor o en el primer montaje.

## Notas relacionadas

- [[index|this — Contexto de invocación]] — explicit binding y prioridades
- [[07 call()]] — invocar con `this` explícito de forma inmediata
- [[08 apply()]] — `call` con argumentos en array
- [[06 Pérdida de this]] — los problemas que `bind` resuelve
- [[03 Clases/04 Propiedades de Instancia|Propiedades de Instancia]] — campo de clase como alternativa a `bind` en constructor
- [[03 Programación Funcional/06 Currying y Composición/02 Partial Application|Partial Application]] — `bind` como mecanismo de partial application
