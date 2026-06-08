---
title: Función async — declarar una función asíncrona
aliases:
  - async function
  - función async
tags:
  - javascript
  - api/concepto
  - asincrono
objeto: Function
tipo: concepto
retorna: Promise
muta: false
asincrono: true
draft: false
---

# Función `async`

> [!definicion]
> La palabra clave `async` declara una función asíncrona. Toda función `async` retorna siempre una `Promise`: si el cuerpo devuelve un valor no-Promise, el motor lo envuelve en `Promise.resolve(valor)`; si lanza una excepción, retorna `Promise.reject(error)`. Dentro de su cuerpo puede usarse `await` para pausar la ejecución hasta que una Promise se asiente.

```js
async function obtenerPerfil(id) {
  const res = await fetch(`/api/users/${id}`);
  return res.json(); // devuelve Promise<Object>, no Object
}

// El retorno siempre es Promise — aunque el valor interno sea plano
obtenerPerfil(1).then(perfil => console.log(perfil.name));
```

## Formas de declaración

| Forma | Sintaxis | Cuándo usarla |
|---|---|---|
| Función declarada | `async function fn() {}` | Hoisting necesario, definición de módulo |
| Expresión de función | `const fn = async function() {}` | Asignación explícita, sin hoisting |
| Arrow function | `const fn = async () => {}` | Callbacks inline, sin `this` propio |
| Método de clase | `async metodo() {}` | Operaciones asíncronas de instancia |
| IIFE async | `(async () => { ... })()` | Top-level await antes de ES2022 |

```js
// Declarada
async function procesarLote(items) { /* ... */ }

// Arrow — la forma más compacta para callbacks
const ids = [1, 2, 3];
const perfiles = await Promise.all(ids.map(async id => {
  const r = await fetch(`/api/users/${id}`);
  return r.json();
}));

// Método de clase
class ServicioUsuario {
  async obtener(id) {
    return fetch(`/api/users/${id}`).then(r => r.json());
  }
}

// IIFE async — patrón legacy para top-level await
(async () => {
  const config = await cargarConfig();
  iniciarApp(config);
})();
```

## Retorno implícito como Promise

El comportamiento del retorno es determinista:

```js
async function a() { return 42; }
// Equivale exactamente a:
function a() { return Promise.resolve(42); }

async function b() { throw new Error('fallo'); }
// Equivale exactamente a:
function b() { return Promise.reject(new Error('fallo')); }

async function c() { return fetch('/api'); }
// El motor NO doble-envuelve: si el return ya es Thenable,
// se adopta su estado directamente (no Promise<Promise<Response>>)
```

> [!info]
> Retornar una Promise desde una función `async` no la anida: `async function f() { return Promise.resolve(1) }` produce una Promise que se resuelve a `1`, no a `Promise<1>`. El mecanismo de "aplanamiento" (assimilation) opera igual que en `.then()`.

## Top-level await (ES2022)

En módulos ES (`<script type="module">` o archivos `.mjs`), `await` se puede usar directamente sin envolver en una función `async`. El módulo pausa su evaluación hasta que la expresión se resuelva, y los módulos que lo importan esperan su resolución.

```js
// config.js — módulo ES
const config = await fetch('/config.json').then(r => r.json());
export const API_URL = config.apiUrl;

// main.js — importa config.js y espera automáticamente
import { API_URL } from './config.js';
console.log(API_URL); // ya resuelto
```

> [!warning]
> Top-level `await` solo funciona en módulos ES. En scripts clásicos (`<script>` sin `type="module"`) o en CommonJS (`require`) lanza `SyntaxError`. En CJS, la alternativa sigue siendo la IIFE async.

## Cómo funciona por dentro

El motor transforma la función `async` en una **máquina de estados** (similar a un generador). Cada `await` marca un punto de suspensión. Al llamar a la función:

1. Se ejecuta el cuerpo síncronamente hasta el primer `await`.
2. Se registra un callback interno en la microtask queue ligado al estado de la Promise awaited.
3. La función retorna su Promise externa (aún `pending`).
4. Cuando la Promise awaited se asienta, el callback reanuda la ejecución desde el punto de suspensión.
5. Si el cuerpo termina (con `return`), la Promise externa se resuelve; si lanza, se rechaza.

```js
// Lo que escribe el desarrollador:
async function suma(a, b) {
  const x = await Promise.resolve(a);
  const y = await Promise.resolve(b);
  return x + y;
}

// Lo que conceptualmente genera el motor (simplificado):
function suma(a, b) {
  return new Promise((resolve, reject) => {
    Promise.resolve(a).then(x => {
      Promise.resolve(b).then(y => {
        resolve(x + y);
      }).catch(reject);
    }).catch(reject);
  });
}
```

> [!tip]
> Declarar métodos `async` en clases es la forma idiomática de encapsular lógica de red o I/O. El `this` se preserva correctamente (a diferencia de pasar `.then()` como callback sin bindear).

## Notas relacionadas

- [[02 await|await]] — el operador que pausa la función async
- [[03 Manejo de Errores (try-catch)|Manejo de Errores con try/catch]] — capturar rechazos de awaits
- [[05 Promesas/index|Promesas]] — el mecanismo subyacente que envuelve el retorno
- [[04 Microtask Queue|Microtask Queue]] — donde se encola la reanudación
