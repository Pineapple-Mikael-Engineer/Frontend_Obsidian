---
title: URIError, EvalError y AggregateError — tipos de error poco frecuentes
aliases:
  - URIError
  - EvalError
  - AggregateError
  - otros errores js
tags:
  - javascript
  - api/clase
  - errores
objeto: Error
tipo: clase
retorna: "-"
muta: false
asincrono: false
draft: false
order: 5
---

# URIError, EvalError y AggregateError

> [!definicion]
> Tres subclases de `Error` con usos acotados: `URIError` cubre las funciones de codificación de URI con secuencias inválidas, `EvalError` es un remanente histórico que los engines modernos raramente emiten, y `AggregateError` (ES2021) agrupa múltiples errores bajo un solo objeto.

## Comparativa rápida

| Tipo | Cuándo se lanza | Frecuencia en producción |
|------|----------------|--------------------------|
| `URIError` | `decodeURIComponent`/`decodeURI` recibe secuencia malformada | Baja — solo con URIs externas sin validar |
| `EvalError` | Históricamente ligado a `eval()` ilegal | Muy baja — engines modernos emiten `TypeError`/`SyntaxError` |
| `AggregateError` | `Promise.any()` cuando todas las promesas rechazan | Media — patrón habitual en cargas paralelas con fallback |

---

## URIError

`URIError` se lanza exclusivamente por `decodeURI()` y `decodeURIComponent()` cuando reciben una secuencia de escape malformada (porcentaje sin dígitos hex válidos, sustituto UTF-16 sin par, etc.).

```js
try {
  decodeURIComponent("%"); // secuencia incompleta
} catch (err) {
  console.log(err instanceof URIError); // → true
  console.log(err.message); // → "URI malformed"
}
```

`encodeURI` y `encodeURIComponent` **no** lanzan `URIError`: escapan cualquier entrada válida en JS sin fallar.

### Wrapper seguro para decodeURIComponent

```js
function decodeSafe(raw, fallback = "") {
  try {
    return decodeURIComponent(raw);
  } catch (err) {
    if (err instanceof URIError) return fallback;
    throw err;
  }
}

decodeSafe("Hola%20mundo"); // → "Hola mundo"
decodeSafe("%");             // → ""  (fallback)
```

Útil al procesar parámetros de URL que vienen de fuentes externas (`location.search`, headers, redirecciones de terceros).

---

## EvalError

`EvalError` se creó para señalar usos ilegales de `eval()` en versiones tempranas de ECMAScript. En la especificación actual, ningún algoritmo de los engines modernos (V8, SpiderMonkey, JavaScriptCore) lo lanza directamente: `eval()` con código inválido lanza `SyntaxError`; un `eval` restringido (CSP) suele lanzar `EvalError` o simplemente bloquear la ejecución sin excepción, dependiendo del navegador.

```js
// EvalError no se genera en condiciones normales de uso moderno:
try {
  eval("{{"); // → SyntaxError, no EvalError
} catch (err) {
  console.log(err.constructor.name); // → "SyntaxError"
}
```

`EvalError` sigue siendo instanciable manualmente pero no tiene un caso de uso práctico como tipo lanzado por el engine. Se conserva en la especificación por compatibilidad.

---

## AggregateError

`AggregateError` (ES2021) agrupa varios errores bajo un único objeto. `Promise.any()` lo usa cuando **todas** las promesas del iterable rechazan: el `AggregateError` resultante contiene la colección de rechazos en `error.errors`.

```js
const promesas = [
  Promise.reject(new Error("fallo A")),
  Promise.reject(new Error("fallo B")),
  Promise.reject(new Error("fallo C")),
];

try {
  await Promise.any(promesas);
} catch (err) {
  console.log(err instanceof AggregateError); // → true
  console.log(err.message);   // → "All promises were rejected"
  console.log(err.errors);
  // → [Error: "fallo A", Error: "fallo B", Error: "fallo C"]
}
```

`err.errors` es un array con los valores de rechazo de cada promesa, en el orden del iterable original.

También se puede instanciar manualmente para agrupar errores de validación:

```js
const erroresValidacion = [
  new Error("Campo email requerido"),
  new Error("Password demasiado corto"),
];
throw new AggregateError(erroresValidacion, "Formulario inválido");
```

---

## Jerarquía completa de Error

```
Error
├── SyntaxError
├── ReferenceError
├── TypeError
├── RangeError
├── URIError
├── EvalError
└── AggregateError   (ES2021)
```

Todos comparten la cadena de prototipos hacia `Error.prototype`. Para crear tipos propios se extiende la clase base: el patrón `class MiError extends Error` con `this.name` asignado manualmente se delega a [[03 throw/index | throw y errores personalizados]].

> [!tip]
> Al capturar `AggregateError`, iterar `err.errors` para reportar cada fallo individualmente en lugar de mostrar solo el mensaje de la excepción envolvente, que es siempre el genérico `"All promises were rejected"`.

> [!warning]
> `Promise.any()` y `AggregateError` requieren soporte ES2021. En entornos legacy (Node < 15, IE) se necesita polyfill. Verificar con `typeof AggregateError !== "undefined"` o confiar en la tabla de compatibilidad del proyecto.

## Notas relacionadas

- [[01 Tipos de Errores/index | Tipos de Errores]]
- [[01 Tipos de Errores/04 RangeError | RangeError]]
- [[03 throw/index | throw y errores personalizados]]
- [[02 try - catch - finally/01 try y catch | try y catch]]
