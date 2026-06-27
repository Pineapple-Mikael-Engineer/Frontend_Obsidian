---
title: Partial Application — Fijar argumentos iniciales de una función
aliases:
  - partial application
  - aplicación parcial
  - partial
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
order: 2
---

# Partial Application

> [!definicion]
> La **partial application** (aplicación parcial) produce una nueva función fijando uno o más argumentos iniciales de una función existente. La función resultante espera los argumentos restantes. A diferencia del currying, la aplicación parcial puede fijar varios argumentos a la vez y la función resultante puede recibir también varios argumentos.

```js
function partial(fn, ...argsIniciales) {
  return function(...argsRest) {
    return fn(...argsIniciales, ...argsRest);
  };
}

const multiplicar = (a, b) => a * b;
const doble = partial(multiplicar, 2);

doble(5);   // 10
doble(9);   // 18
doble(0.5); // 1
```

## Con `Function.prototype.bind`

`bind` es la forma nativa de partial application en JavaScript. El primer argumento fija `this`; los siguientes fijan los argumentos posicionales.

```js
function log(nivel, prefijo, mensaje) {
  console.log(`[${nivel}] ${prefijo}: ${mensaje}`);
}

const errorLog = log.bind(null, "ERROR");
const warnDB   = log.bind(null, "WARN", "DB");

errorLog("Auth", "Token inválido");     // [ERROR] Auth: Token inválido
warnDB("Conexión lenta");               // [WARN] DB: Conexión lenta
```

El primer argumento `null` es el `this` — en funciones puras no importa. `bind` es simple pero solo permite fijar argumentos desde el principio en orden posicional.

## Implementación manual completa

```js
function partial(fn, ...argsIniciales) {
  return function(...argsRest) {
    return fn(...argsIniciales, ...argsRest);
  };
}

// Configurar una función de fetch con la URL base
const fetchBase = (base, endpoint, opciones) =>
  fetch(`${base}${endpoint}`, opciones);

const fetchAPI = partial(fetchBase, "https://api.ejemplo.com");

fetchAPI("/usuarios", { method: "GET" });
fetchAPI("/productos", { method: "POST", body: JSON.stringify({ nombre: "x" }) });
```

## Recetas de uso

### Logger con nivel y prefijo configurables

```js
function registrar(nivel, modulo, mensaje) {
  const ts = new Date().toISOString();
  console.log(`${ts} [${nivel}] [${modulo}] ${mensaje}`);
}

const infoAuth  = partial(registrar, "INFO", "Auth");
const errorDB   = partial(registrar, "ERROR", "DB");

infoAuth("Usuario autenticado");     // 2024-... [INFO] [Auth] Usuario autenticado
errorDB("Timeout en consulta");      // 2024-... [ERROR] [DB] Timeout en consulta
```

### Validadores parametrizados

```js
function validarRango(min, max, valor) {
  return valor >= min && valor <= max;
}

const entre0y100   = partial(validarRango, 0, 100);
const entreNeg1y1  = partial(validarRango, -1, 1);

entre0y100(75);    // true
entre0y100(120);   // false
entreNeg1y1(0.5);  // true
```

### Transformador de objetos configurable

```js
function proyectar(campos, objeto) {
  return campos.reduce((acc, campo) => {
    if (campo in objeto) acc[campo] = objeto[campo];
    return acc;
  }, {});
}

const soloPublico  = partial(proyectar, ["id", "nombre", "avatar"]);
const soloFinanzas = partial(proyectar, ["id", "sueldo", "departamento"]);

const usuario = { id: 1, nombre: "Ana", email: "ana@x.com", sueldo: 3000, avatar: "ana.jpg" };

soloPublico(usuario);   // { id: 1, nombre: "Ana", avatar: "ana.jpg" }
soloFinanzas(usuario);  // { id: 1, sueldo: 3000, departamento: undefined }
```

## Partial application con placeholder

Para fijar argumentos en posiciones no-iniciales se puede usar un símbolo `_` como marcador de posición:

```js
const _ = Symbol("placeholder");

function partialWith(fn, ...argsConPlaceholders) {
  return function(...argsRest) {
    const args = [...argsConPlaceholders];
    let restIdx = 0;
    for (let i = 0; i < args.length; i++) {
      if (args[i] === _) args[i] = argsRest[restIdx++];
    }
    return fn(...args, ...argsRest.slice(restIdx));
  };
}

const dividir = (a, b) => a / b;
const mitad = partialWith(dividir, _, 2);  // fija b=2, a queda libre

mitad(10);  // 5
mitad(30);  // 15
```

## Diferencia con currying

| Aspecto | Partial Application | Currying |
|---|---|---|
| Argumentos fijados por llamada | Varios posibles | Uno a la vez (unaria) |
| Aridad del resultado | Cualquiera | Siempre 1 |
| Posición de los fijados | Inicial (o con placeholder) | Izquierda a derecha en orden |
| Propósito principal | Configurar una función reutilizable | Secuencia de especializaciones unarias |

## Cómo funciona por dentro

La partial application es un closure: la función `partial` devuelve una nueva función que **captura** `argsIniciales` en su ámbito. Cuando la función resultante se invoca con `argsRest`, combina ambos arrays y llama a `fn` con todos los argumentos. No hay mecanismo especial del lenguaje — solo closures y spread.

```js
// El closure captura argsIniciales
const doble = partial(multiplicar, 2);
// internamente: () => multiplicar(2, ...argsRest)

doble(5);
// multiplicar(2, 5) → 10
```

> [!tip] Buenas prácticas
> - Diseñar funciones con los argumentos de configuración primero y los datos de entrada al final, para que la partial application sea natural: `fn(config)(dato)`.
> - Para casos simples con argumento inicial único, `bind(null, arg)` es suficiente y no requiere una función `partial` auxiliar.
> - La partial application es especialmente potente al combinarse con `map`, `filter` y `compose`/`pipe`: crear la función especializada una vez y reutilizarla en todo el pipeline.

> [!warning] Errores comunes
> - **Fijar `this` incorrectamente con `bind`:** `metodo.bind(null, arg)` pierde el `this` del objeto. Si el método necesita `this`, usar `metodo.bind(objeto, arg)`.
> - **Confundir el orden de los argumentos:** en la partial application, los argumentos fijados van al principio (izquierda). Si la función espera los datos antes que la configuración, la partial no resulta natural.
> - **Asumir que `fn.length` refleja los argumentos restantes:** `fn.bind(null, a).length` devuelve `fn.length - 1`, pero solo si `fn` no tiene parámetros rest ni defaults. No confiar en `.length` para lógica dinámica.

## Notas relacionadas

- [[index|Currying y Composición — Índice]]
- [[01 Currying]]
- [[03 compose]]
- [[04 pipe]]
