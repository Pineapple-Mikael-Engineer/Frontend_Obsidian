---
title: Arrow Functions — Cuándo usarlas y cuándo no
aliases:
  - cuándo usar arrow functions
  - arrow vs función regular
  - decidir arrow o function
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
---

# Cuándo Usarlas

> [!definicion]
> La elección entre arrow function y función regular es **semántica, no estética**. La regla operativa: usa arrow cuando el `this` léxico es correcto para el contexto, cuando la función no actúa como constructora, generadora ni necesita `super`, y cuando la concisión mejora la legibilidad. Usa función regular cuando el `this` dinámico es necesario, cuando la función es un método de objeto, o cuando se requiere alguna de las capacidades que las arrows no tienen.

## Tabla de decisión

| Contexto de uso | Arrow | Función regular | Razón principal |
|---|---|---|---|
| Callback de `map`/`filter`/`reduce` | Preferida | Funciona | Concisión; `this` raramente importa |
| Callback de `sort` con comparador | Preferida | Funciona | Expresión de una línea |
| Callback en `setTimeout`/`setInterval` dentro de método | Preferida | Requiere `.bind` o variable | Hereda `this` léxico del método |
| Handler de evento pasado a API externa | Campo arrow en clase | Requiere `.bind(this)` | `this` garantizado sin bind manual |
| Método de objeto literal | No | Sí | Arrow no ve el objeto como `this` |
| Método de clase regular | No (como campo, sí si se acepta el coste) | Sí | Prototype sharing; puede usar `super` |
| Función constructora | No | Sí / `class` preferido | Arrow lanza TypeError con `new` |
| Función generadora | No | Sí (`function*`) | Sintaxis no permitida |
| Función que necesita `arguments` dinámico | No | Sí | Arrow no tiene `arguments` propio |
| Método que usa `super` | No (campo) | Sí | Arrow carece de slot `[[HomeObject]]` |
| Composición funcional (`pipe`/`compose`) | Preferida | Funciona | Unarias concisas encajan directamente |
| Función recursiva nombrada | No recomendada | Sí | La arrow no tiene nombre propio fiable para la recursión interna |

## Cuándo SÍ usar arrows

### 1. Callbacks de métodos de array

El uso más común y seguro. La concisión del return implícito hace el código declarativo:

```js
const activos  = usuarios.filter(u => u.activo);
const nombres  = activos.map(u => u.nombre);
const total    = pedidos.reduce((acc, p) => acc + p.precio, 0);
const ordenado = [...items].sort((a, b) => a.nombre.localeCompare(b.nombre));
```

### 2. Callbacks asíncronos dentro de métodos

Cuando un método necesita `this` dentro de un callback asíncrono, la arrow captura el contexto correcto sin `.bind`:

```js
class Repositorio {
  constructor(api) { this.api = api; this.cache = {}; }

  cargarTodos() {
    return fetch(this.api)              // this = instancia
      .then(r => r.json())              // arrow — this heredado
      .then(datos => {
        this.cache = datos;             // this = instancia, correcto
        return datos;
      });
  }
}
```

### 3. Composición funcional

Las arrows unarias son las piezas naturales de `pipe` y `compose`:

```js
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);

const procesar = pipe(
  str => str.trim(),
  str => str.toLowerCase(),
  str => str.replace(/\s+/g, "-"),
  str => encodeURIComponent(str),
);

procesar("  Hola Mundo  "); // → "hola-mundo"
```

### 4. Función de una sola expresión

Cuando la función es una transformación directa, el return implícito mejora la densidad informativa:

```js
const esPar       = n  => n % 2 === 0;
const capitalizar = s  => s.charAt(0).toUpperCase() + s.slice(1);
const clamp       = (v, min, max) => Math.min(Math.max(v, min), max);
const invertir    = obj => Object.fromEntries(Object.entries(obj).map(([k, v]) => [v, k]));
```

### 5. Handlers de eventos en componentes de clase

Declarar el handler como campo arrow garantiza que `this` siempre sea la instancia, eliminando la necesidad de `bind` en el constructor:

```js
class FormularioLogin {
  constructor() {
    this.intentos = 0;
    document.querySelector("#login").addEventListener("click", this.handleSubmit);
  }

  // Campo arrow — this siempre = instancia de FormularioLogin
  handleSubmit = (e) => {
    e.preventDefault();
    this.intentos++;
    this.validar();
  };

  validar() { /* ... */ }
}
```

## Cuándo NO usar arrows

### 1. Métodos de objeto literal

```js
// MAL — this no es persona
const persona = {
  nombre: "Ana",
  saludar: () => `Hola, soy ${this.nombre}`, // this = scope global/module
};

// BIEN — this es persona
const persona = {
  nombre: "Ana",
  saludar() { return `Hola, soy ${this.nombre}`; },
};
```

### 2. Funciones constructoras

```js
// MAL — TypeError en tiempo de ejecución
const Punto = (x, y) => { this.x = x; this.y = y; };
new Punto(1, 2); // TypeError

// BIEN
class Punto {
  constructor(x, y) { this.x = x; this.y = y; }
}
```

### 3. Métodos de prototipo con super

```js
// MAL — campo arrow no tiene [[HomeObject]]
class Perro extends Animal {
  hablar = () => super.hablar() + " guau"; // TypeError
}

// BIEN — método de clase con [[HomeObject]]
class Perro extends Animal {
  hablar() { return super.hablar() + " guau"; }
}
```

### 4. Funciones que necesitan `arguments` dinámico

```js
// MAL — arguments no existe en arrow (hereda del exterior o ReferenceError)
const logger = () => {
  Array.from(arguments).forEach(a => console.log(a)); // puede fallar
};

// BIEN — rest params o función regular
const logger = (...args) => args.forEach(a => console.log(a));
```

### 5. Funciones generadoras

```js
// MAL — SyntaxError
const rango = *() => { /* ... */ };

// BIEN
function* rango(inicio, fin) {
  for (let i = inicio; i <= fin; i++) yield i;
}
```

## Receta: patrón "todos los handlers como campos arrow"

Común en componentes de clase de React pre-hooks y en clases de UI imperativa. Todos los handlers se declaran como campos arrow para no necesitar `.bind(this)` en el constructor:

```js
class Componente {
  constructor(elemento) {
    this.elemento   = elemento;
    this.conteo     = 0;
    // Sin bind — los handlers ya tienen this correcto
    elemento.addEventListener("click",     this.handleClick);
    elemento.addEventListener("mouseenter", this.handleHover);
    elemento.addEventListener("keydown",   this.handleKey);
  }

  handleClick = () => {
    this.conteo++;
    this.render();
  };

  handleHover = (e) => {
    this.elemento.classList.toggle("hover", e.type === "mouseenter");
  };

  handleKey = (e) => {
    if (e.key === "Enter") this.handleClick();
  };

  render() {
    this.elemento.textContent = `Pulsado ${this.conteo} veces`;
  }

  destruir() {
    // removeEventListener funciona porque handleClick es la misma referencia
    this.elemento.removeEventListener("click",     this.handleClick);
    this.elemento.removeEventListener("mouseenter", this.handleHover);
    this.elemento.removeEventListener("keydown",   this.handleKey);
  }
}
```

**Ventaja clave:** `removeEventListener` requiere la **misma referencia** de función que `addEventListener`. Con campos arrow, cada instancia tiene su propia referencia estable — a diferencia de `bind`, que crea una nueva función en cada llamada (lo que haría que `removeEventListener` no encontrara el handler).

**Inconveniente:** una copia de cada método en cada instancia, en lugar de compartir en `prototype`. En clases con pocas instancias y muchos handlers el coste es insignificante; con miles de instancias, vale la pena medir.

## Cómo funciona por dentro

No existe coste de rendimiento medible entre invocar una arrow y una función regular en motores modernos (V8, SpiderMonkey, JavaScriptCore) — ambas pasan por los mismos pipelines de compilación JIT. La decisión es puramente semántica. Las optimizaciones de inline y monomorphic call-site se aplican igual en ambos tipos. El único coste real de las arrows es el de campo de clase: en lugar de una entrada en el prototype object compartida, se crea una propiedad propia en cada instancia durante `[[Construct]]`, con el consiguiente uso de memoria proporcional al número de instancias.

> [!tip] Buenas prácticas
> - Adopta arrows como el formato por defecto para callbacks y expresiones de función cortas — reserva `function` para los casos donde la semántica lo exige.
> - Cuando pases un método como callback a una API externa, usa campo arrow en la clase para evitar `.bind` y garantizar que `removeEventListener` pueda encontrar la misma referencia.
> - En composición funcional, mantén las arrows unarias sin paréntesis innecesarios: `x => x * 2` es más legible que `(x) => { return x * 2; }`.
> - Si una arrow deja de ser "pequeña" (más de 3-4 líneas de lógica real), extráela como función nombrada para mejorar los stack traces.

> [!warning] Errores comunes
> - Usar arrows para métodos de objetos literales porque "es más moderno" — el bug de `this` aparece en runtime y puede ser difícil de rastrear.
> - Pasar `this.metodo.bind(this)` a `addEventListener` y luego intentar `removeEventListener` con `this.metodo.bind(this)` — cada `bind` crea una nueva función, por lo que el remove no encuentra el handler. Guardar la referencia del bind o usar campo arrow.
> - Asumir que arrow es siempre más eficiente — en clases con muchas instancias, el campo arrow tiene coste de memoria real.
> - Escribir arrows de una línea muy densas que sacrifican legibilidad por brevedad: `const f = a => b => c => a + b + c` puede ser elegante, pero también opaco para quien lee el código por primera vez.

## Notas relacionadas

- [[01 Sintaxis y Return Implícito]] — referencia de sintaxis y formas de parámetros
- [[02 Sin this Propio]] — detalle del binding léxico de `this`
- [[03 Sin arguments ni new]] — detalle de las limitaciones sin `arguments`, `new` y `super`
- [[01 Conceptos Fundamentales/04 Composición de Funciones|Composición de Funciones]] — context de uso de arrows en `pipe`/`compose`
- [[02 Funciones de Orden Superior/index|Funciones de Orden Superior]] — las arrows como callbacks idiomáticos de las HOFs
