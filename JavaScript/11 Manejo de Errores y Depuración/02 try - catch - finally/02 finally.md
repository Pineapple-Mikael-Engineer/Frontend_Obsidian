---
title: finally — ejecución garantizada independiente del resultado
aliases:
  - finally js
  - bloque finally
tags:
  - javascript
  - api/concepto
  - errores
objeto: "-"
tipo: concepto
retorna: "-"
muta: false
asincrono: false
draft: false
---

# finally

> [!definicion]
> El bloque `finally` se ejecuta **siempre** después de `try` y `catch`, sin importar si el código terminó normalmente, si lanzó un error, o si alguno de los bloques anteriores ejecutó un `return`, `break` o `continue`. Es el mecanismo para garantizar limpieza de recursos o restauración de estado.

```js
function leerDatos(fuente) {
  fuente.abrir();
  try {
    return fuente.leer();
  } catch (err) {
    console.error("Error al leer:", err.message);
    return null;
  } finally {
    fuente.cerrar(); // siempre se ejecuta, incluso con el return de try
  }
}
```

## Tabla de comportamiento

| Escenario | Qué ejecuta | Qué se retorna / propaga |
|-----------|-------------|--------------------------|
| `try` termina sin error | `try` → `finally` | valor de `return` en `try` |
| `try` lanza, `catch` maneja | `try` (parcial) → `catch` → `finally` | valor de `return` en `catch` |
| `try` lanza, sin `catch` | `try` (parcial) → `finally` | el error se propaga tras `finally` |
| `catch` lanza | `try` (parcial) → `catch` (parcial) → `finally` | el error de `catch` se propaga |
| `try` tiene `return` | `try` (hasta return) → `finally` | valor del `return` de `try` (salvo que `finally` tenga `return`) |
| `finally` tiene `return` | `try`/`catch` → `finally` (con return) | **valor del `return` de `finally`** — anula el de `try`/`catch` |
| `finally` lanza | `try`/`catch` → `finally` (parcial, lanza) | el error de `finally` **reemplaza** el error original |

## finally anula el return del try

```js
function demo() {
  try {
    return "desde try";
  } finally {
    return "desde finally"; // este gana
  }
}
demo(); // → "desde finally"
```

Este comportamiento es una trampa común. Evitar `return` en `finally` salvo que la intención sea forzar un valor de retorno independientemente de lo que haya ocurrido.

## finally no recibe información del error

`finally` no tiene parámetros y no sabe si hubo error ni cuál fue. Para tomar decisiones distintas según si hubo error, usar una variable de flag:

```js
async function operacion() {
  let exito = false;
  try {
    await realizarTarea();
    exito = true;
  } catch (err) {
    reportarError(err);
  } finally {
    ocultarSpinner();
    if (exito) actualizarUI();
  }
}
```

## Cuándo usar finally

| Caso de uso | Ejemplo |
|-------------|---------|
| Cerrar conexiones de BD | `conexion.close()` en finally |
| Ocultar spinner de carga de UI | `setLoading(false)` |
| Liberar locks o semáforos | `mutex.release()` |
| Restaurar estado temporal | `document.body.style.cursor = ""` |
| Decrementar contadores de peticiones en vuelo | `--contadorPeticionesActivas` |

## Equivalente en Promises

`.finally()` en una Promise tiene la misma semántica: se ejecuta cuando la promesa se asienta (resuelve o rechaza), recibe el valor/error original sin modificarlo, y si el callback de `.finally()` retorna una promesa rechazada, esa reemplaza el resultado original.

```js
fetch("/api/datos")
  .then(res => res.json())
  .catch(err => console.error(err))
  .finally(() => setLoading(false)); // siempre oculta el spinner
```

La correspondencia es directa: `finally { }` en `try/catch` ↔ `.finally(() => { })` en la cadena de promesas.

> [!tip]
> El patrón `try { ... } finally { limpiar() }` sin `catch` es completamente válido. Útil cuando no se quiere silenciar el error (debe propagarse) pero sí garantizar que el recurso se libera. El error pasa, la limpieza ocurre.

> [!warning]
> Si `finally` lanza, el error original de `try` se **pierde**: queda silenciado por el error de `finally`. En código de infraestructura (cierre de conexiones, liberación de recursos), proteger el código de limpieza con su propio `try/catch` interno para evitar este efecto.

## Notas relacionadas

- [[02 try - catch - finally/index | try / catch / finally]]
- [[02 try - catch - finally/01 try y catch | try y catch]]
- [[03 throw/index | throw y errores personalizados]]
