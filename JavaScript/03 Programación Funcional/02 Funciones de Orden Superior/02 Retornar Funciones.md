---
title: Retornar Funciones
aliases:
  - HOF retornar
  - factory de funciones
  - function factory
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
---

# Retornar Funciones

> [!definicion]
> Una HOF que **devuelve una función** produce un **closure**: la función devuelta captura el ámbito léxico de la función exterior y mantiene acceso a sus variables incluso después de que la función exterior haya retornado. Este mecanismo permite crear funciones especializadas, acumular estado privado y envolver comportamiento existente sin modificarlo.

```js
function crearMultiplicador(factor) {
  // "factor" queda capturado en el closure de la función devuelta
  return n => n * factor;
}

const doble  = crearMultiplicador(2);
const triple = crearMultiplicador(3);

doble(5);   // 10
triple(5);  // 15
doble(7);   // 14
```

## Casos de uso fundamentales

| Patrón | Qué hace | Cuándo usarlo |
|---|---|---|
| Factory de funciones | Crea variantes especializadas de una función general | Configurar comportamiento en tiempo de creación |
| Memoización | Cachea resultados de llamadas previas | Funciones puras costosas con argumentos repetidos |
| Currying | Convierte `f(a,b)` en `f(a)(b)` | Composición y partial application |
| Decoradores | Envuelve una función para añadir comportamiento | Logging, timing, retry, validación |
| Partial application | Fija algunos argumentos de antemano | Especializar funciones genéricas |

## Factory de funciones

Una factory toma parámetros de configuración y devuelve una función ya configurada. Evita repetir la configuración en cada llamada:

```js
function crearSaludador(saludo, puntuacion = "!") {
  return nombre => `${saludo}, ${nombre}${puntuacion}`;
}

const hola    = crearSaludador("Hola");
const buenos  = crearSaludador("Buenos días", ".");
const formal  = crearSaludador("Estimado/a", ":");

hola("Ana");    // "Hola, Ana!"
buenos("Luis"); // "Buenos días, Luis."
formal("Dr. García"); // "Estimado/a, Dr. García:"
```

Las factories también sirven para encapsular lógica de construcción compleja:

```js
function crearContador(inicio = 0, paso = 1) {
  let valor = inicio;
  return {
    siguiente: () => (valor += paso),
    actual:    () => valor,
    reset:     () => { valor = inicio; },
  };
}

const c = crearContador(10, 5);
c.siguiente(); // 15
c.siguiente(); // 20
c.actual();    // 20
c.reset();
c.actual();    // 10
```

## Memoización

Cachea el resultado de llamadas anteriores usando la serialización de los argumentos como clave. Solo es correcta para funciones **puras** (sin efectos laterales y con salida determinística):

```js
function memoize(fn) {
  const cache = new Map();
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const resultado = fn.apply(this, args);
    cache.set(key, resultado);
    return resultado;
  };
}

// Fibonacci sin memo: O(2^n)
// Con memo: O(n)
const fib = memoize(function fibonacci(n) {
  if (n <= 1) return n;
  return fib(n - 1) + fib(n - 2);
});

fib(40); // rápido
fib(40); // instantáneo (del cache)
```

Limitación de `JSON.stringify`: no serializa `undefined`, funciones, ni objetos circulares. Para esos casos se necesita una clave personalizada.

## Currying

Transforma una función de múltiples argumentos en una cadena de funciones de un argumento:

```js
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    }
    return function(...masArgs) {
      return curried.apply(this, args.concat(masArgs));
    };
  };
}

const sumar = curry((a, b, c) => a + b + c);

sumar(1)(2)(3);    // 6
sumar(1, 2)(3);    // 6
sumar(1)(2, 3);    // 6
sumar(1, 2, 3);    // 6

// Partial application natural con curry
const sumarCinco = sumar(5);
sumarCinco(3)(2); // 10
```

## Decoradores

Un decorador toma una función y devuelve otra con el mismo comportamiento observable más funcionalidad añadida:

### Logging

```js
function conLog(fn) {
  return function(...args) {
    console.log(`→ ${fn.name}(${args.join(", ")})`);
    const resultado = fn.apply(this, args);
    console.log(`← ${fn.name} devuelve:`, resultado);
    return resultado;
  };
}

function sumar(a, b) { return a + b; }
const sumarLog = conLog(sumar);

sumarLog(3, 4);
// → sumar(3, 4)
// ← sumar devuelve: 7
```

### Timing

```js
function conTiming(fn) {
  return function(...args) {
    const inicio = performance.now();
    const resultado = fn.apply(this, args);
    const fin = performance.now();
    console.log(`${fn.name}: ${(fin - inicio).toFixed(2)}ms`);
    return resultado;
  };
}
```

### Retry con límite

```js
function conRetry(fn, intentos = 3) {
  return async function(...args) {
    for (let i = 0; i < intentos; i++) {
      try {
        return await fn(...args);
      } catch (e) {
        if (i === intentos - 1) throw e;
        console.warn(`Intento ${i + 1} fallido. Reintentando...`);
      }
    }
  };
}

const fetchConRetry = conRetry(fetch, 3);
const datos = await fetchConRetry("https://api.ejemplo.com/datos");
```

## Partial application

Fija algunos argumentos de una función y devuelve una versión que espera los restantes:

```js
function partial(fn, ...argsParciales) {
  return function(...argsRestantes) {
    return fn(...argsParciales, ...argsRestantes);
  };
}

function potencia(base, exponente) {
  return base ** exponente;
}

const cuadrado = partial(potencia, 2);   // base=2 fija
const cubo     = partial(potencia, 3);   // base=3 fija

cuadrado(8); // 256  (2^8)
cubo(4);     // 81   (3^4)
```

## Recetas comunes

### once(fn)

Ejecuta `fn` solo la primera vez; en llamadas posteriores devuelve el mismo resultado sin ejecutar `fn` de nuevo:

```js
function once(fn) {
  let llamado   = false;
  let resultado;
  return function(...args) {
    if (!llamado) {
      llamado   = true;
      resultado = fn.apply(this, args);
    }
    return resultado;
  };
}

const cargarConfig = once(async () => {
  const res = await fetch("/config.json");
  return res.json();
});

// Múltiples módulos llaman a cargarConfig(): solo una petición real
const cfg1 = await cargarConfig();
const cfg2 = await cargarConfig(); // del closure, sin fetch
```

### throttle(fn, ms)

Limita la frecuencia de ejecución de `fn` a una vez por ventana de tiempo:

```js
function throttle(fn, ms) {
  let ultimaEjecucion = 0;
  return function(...args) {
    const ahora = Date.now();
    if (ahora - ultimaEjecucion >= ms) {
      ultimaEjecucion = ahora;
      return fn.apply(this, args);
    }
  };
}

const onScroll = throttle(() => {
  console.log("posición:", window.scrollY);
}, 200);

window.addEventListener("scroll", onScroll);
```

### Composición de decoradores

Los decoradores se apilan combinando HOFs:

```js
function sumar(a, b) { return a + b; }

const sumarMejorado = conLog(conTiming(memoize(sumar)));
// Aplica: memoize primero, timing encima, log al exterior
sumarMejorado(3, 4); // log + tiempo + cache
```

## Cómo funciona por dentro

Cuando JavaScript ejecuta una función que devuelve otra función, el motor crea un **closure**: un objeto interno que empaqueta la función devuelta junto con una referencia a su `[[Environment]]` — el ámbito léxico donde fue definida.

```
crearMultiplicador(3) ejecutado:
  ┌─────────────────────────────────┐
  │  Activation Record              │
  │  factor = 3                     │
  │  return n => n * factor  ──┐    │
  └────────────────────────────┼────┘
                               │  [[Environment]] apunta aquí
                               ▼
                   La función devuelta "triple"
                   lleva consigo esa referencia
```

El Garbage Collector no puede liberar el Activation Record de `crearMultiplicador` mientras exista una referencia a `triple`, porque `triple` lo necesita para resolver `factor`. Cuando `triple = null`, el GC puede recuperar ambos.

Este mecanismo tiene implicaciones prácticas:

- **Memoria**: los closures que capturan objetos grandes pueden retenerlos en memoria más tiempo del esperado.
- **Estado compartido inesperado**: dos closures creados en el mismo ámbito comparten el mismo entorno y ven los cambios del otro.

```js
// Bug clásico: closures en bucle comparten la misma variable
const fns = [];
for (var i = 0; i < 3; i++) {
  fns.push(() => i);   // todas capturan la misma "i"
}
fns[0](); // 3  (no 0!)
fns[1](); // 3
fns[2](); // 3

// Solución: let crea un binding nuevo por iteración
for (let i = 0; i < 3; i++) {
  fns.push(() => i);
}
fns[0](); // 0
fns[1](); // 1
fns[2](); // 2
```

> [!tip] Buenas prácticas
> - Usa decoradores para añadir comportamiento transversal (logging, caché, retry) sin modificar las funciones originales.
> - Prefiere `let`/`const` sobre `var` en bucles que crean closures para evitar el bug de la variable compartida.
> - En factories que devuelven objetos con métodos, ten en cuenta que todos los métodos comparten el mismo closure — eso es intencional para encapsular estado privado.
> - La memoización solo es segura con funciones puras; nunca memoices funciones con efectos laterales o que dependan de estado externo mutable.

> [!warning] Errores comunes
> - **Retener closures innecesariamente**: un listener de evento que captura un objeto grande impide su recolección. Elimina los listeners con `removeEventListener` cuando ya no se necesiten.
> - **Memoizar funciones impuras**: si `fn` tiene efectos laterales o depende de estado externo, la memoización devuelve resultados stale sin ejecutar el efecto.
> - **`this` incorrecto al devolver funciones flecha desde métodos**: las arrow functions no tienen `this` propio; capturan el `this` del ámbito donde se definen, lo que puede ser inesperado en clases.
> - **Confundir partial application con currying**: partial fija _algunos_ argumentos y llama a la función cuando recibe el resto; currying siempre retorna una función de un argumento hasta tener todos.

## Notas relacionadas

- [[index]]
- [[01 Recibir Funciones (callbacks)]]
