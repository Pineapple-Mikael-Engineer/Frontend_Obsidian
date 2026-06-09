---
title: Métodos de console
aliases:
  - console api
  - console.log
  - métodos consola javascript
tags:
  - javascript
  - api/concepto
  - errores
objeto: console
tipo: objeto-global
retorna: undefined
muta: false
asincrono: false
draft: false
---

# Métodos de console

> [!definicion]
> `console` es un objeto global que expone métodos para enviar mensajes al panel de consola del entorno (DevTools del navegador, terminal de Node). Los mensajes se clasifican por nivel (log, info, warn, error, debug) y DevTools permite filtrarlos. `console` no es parte del estándar ECMAScript — pertenece a la Living Standard de WHATWG para navegadores y la API de Node.

## Referencia rápida

| Método | Nivel / Color DevTools | Propósito principal |
|--------|------------------------|---------------------|
| `console.log(...valores)` | Log / sin color | Logging general multipropósito |
| `console.info(...)` | Info / azul (algunos browsers) | Mensajes informativos |
| `console.warn(...)` | Warning / amarillo | Advertencias no fatales |
| `console.error(...)` | Error / rojo + stack | Errores; incluye stack trace en algunos engines |
| `console.debug(...)` | Verbose / gris | Solo visible con nivel "Verbose" en DevTools |
| `console.table(datos)` | Log | Array u objeto como tabla interactiva |
| `console.dir(obj)` | Log | Objeto expandible con propiedades JS |
| `console.group(label)` | — | Inicio de grupo con indentación |
| `console.groupCollapsed(label)` | — | Grupo colapsado por defecto |
| `console.groupEnd()` | — | Cierre del grupo activo |
| `console.time(label)` | — | Inicia temporizador con etiqueta |
| `console.timeLog(label)` | — | Imprime tiempo transcurrido sin detener |
| `console.timeEnd(label)` | — | Detiene temporizador e imprime duración |
| `console.count(label)` | — | Cuenta invocaciones con esa etiqueta |
| `console.countReset(label)` | — | Reinicia el contador de la etiqueta |
| `console.assert(cond, msg)` | Error / rojo | Loguea solo si `cond` es falsy |
| `console.trace()` | — | Imprime el stack trace actual |
| `console.clear()` | — | Limpia la consola |

## log, warn, error, info, debug

```js
console.log("Valor:", x, "tipo:", typeof x);  // múltiples argumentos
console.warn("API obsoleta — usa fetchV2");
console.error("Fallo al guardar:", err);       // err.stack visible en DevTools
console.info("Servidor escuchando en :3000");
console.debug("Iteración:", i, "estado:", estado); // solo con Verbose activo
```

`console.log` acepta cualquier número de argumentos; los imprime separados por espacio. Los objetos se muestran expandibles en DevTools.

## Formateo con especificadores

`console.log` soporta especificadores de formato al estilo `printf` cuando el primer argumento es un string:

| Especificador | Sustituye por |
|---------------|--------------|
| `%s` | String |
| `%d` / `%i` | Entero |
| `%f` | Flotante |
| `%o` | Objeto (expandible) |
| `%O` | Objeto (formato estricto) |
| `%c` | Aplica CSS al texto que sigue |

```js
console.log("Usuario %s tiene %d items", nombre, items.length);
console.log("%cATENCIÓN%c texto normal", "color:red;font-weight:bold", "");
```

## console.table

Convierte arrays de objetos en una tabla interactiva con columnas clicables para ordenar:

```js
const usuarios = [
  { id: 1, nombre: "Ana", rol: "admin" },
  { id: 2, nombre: "Luis", rol: "editor" },
];
console.table(usuarios);
// ┌─────────┬────┬────────┬──────────┐
// │ (index) │ id │ nombre │ rol      │
// ├─────────┼────┼────────┼──────────┤
// │    0    │  1 │ "Ana"  │ "admin"  │
// │    1    │  2 │ "Luis" │ "editor" │
// └─────────┴────┴────────┴──────────┘
```

Acepta un segundo argumento con las columnas a mostrar: `console.table(usuarios, ["nombre", "rol"])`.

## console.dir vs console.log para DOM

```js
const btn = document.querySelector("button");
console.log(btn);  // muestra el HTML del nodo: <button class="primary">Enviar</button>
console.dir(btn);  // muestra el objeto JS con todas sus propiedades: accessKey, classList, ...
```

`console.dir` es el método correcto cuando se quiere inspeccionar las propiedades JS de un nodo DOM, no su representación HTML.

## Agrupación

```js
console.group("Petición GET /api/users");
console.log("Headers:", headers);
console.log("Body:", body);
console.groupCollapsed("Datos completos");
console.log(respuestaCompleta);
console.groupEnd(); // cierra "Datos completos"
console.groupEnd(); // cierra "Petición GET /api/users"
```

Los grupos anidados se renderizan con indentación progresiva. `groupCollapsed` parte colapsado — útil para datos verbosos que rara vez se necesitan.

## Medición de tiempo

```js
console.time("sort");
datos.sort((a, b) => a.valor - b.valor);
console.timeEnd("sort"); // → sort: 2.34ms

// Comparar dos implementaciones
console.time("quicksort");
quicksort(arr1);
console.timeEnd("quicksort"); // → quicksort: 1.12ms

console.time("mergesort");
mergesort(arr2);
console.timeEnd("mergesort"); // → mergesort: 0.87ms
```

Las etiquetas de `time`/`timeEnd` deben coincidir exactamente (case-sensitive). `timeLog` imprime el tiempo parcial sin detener el temporizador.

## console.assert

```js
function dividir(a, b) {
  console.assert(b !== 0, "División por cero: b =", b);
  return a / b;
}
dividir(10, 0);
// Assertion failed: División por cero: b = 0
```

Solo produce salida cuando la condición es `false` o falsy. Es una forma compacta de detectar invariantes sin lanzar excepciones.

## console.trace

```js
function c() { console.trace("punto de traza"); }
function b() { c(); }
function a() { b(); }
a();
// Trace: punto de traza
//     at c (<anonymous>:1:12)
//     at b (<anonymous>:2:12)
//     at a (<anonymous>:3:12)
```

Útil para entender desde dónde se está llamando una función sin necesidad de abrir el debugger.

> [!tip]
> En producción, eliminar o silenciar los `console.log` de depuración. Una estrategia común: reemplazar `console` por una librería de logging (p. ej. `pino`, `winston`) que respeta niveles y permite deshabilitar el output en producción con una variable de entorno.

> [!warning]
> Los objetos mostrados en `console.log` se evalúan en el momento de la inspección en DevTools, no en el momento del log. Si el objeto muta después del `console.log`, DevTools puede mostrar el valor modificado. Para capturar el estado en el momento exacto, usar `console.log(JSON.stringify(obj))` o `structuredClone(obj)`.

## Notas relacionadas

- [[04 Depuración/02 debugger y Breakpoints | debugger y Breakpoints]]
- [[04 Depuración/03 Step y Watch Expressions | Step y Watch Expressions]]
