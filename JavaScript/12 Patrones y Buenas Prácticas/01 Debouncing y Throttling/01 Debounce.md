---
title: Debounce — Ejecutar solo al cesar la actividad
aliases:
  - debounce
  - debouncear
tags:
  - javascript
  - api/concepto
  - patrones
draft: false
order: 1
---

# Debounce

> [!definicion]
> **Debounce** envuelve una función `fn` y retrasa su ejecución hasta que haya transcurrido un período de inactividad `delay` ms desde la última llamada. Cada nueva invocación cancela el timer anterior y arranca uno nuevo. El resultado es que `fn` solo se ejecuta cuando el flujo de llamadas se detiene.

```js
function debounce(fn, delay) {
  let timerId;
  return function(...args) {
    clearTimeout(timerId);
    timerId = setTimeout(() => fn.apply(this, args), delay);
  };
}

// Uso — búsqueda en tiempo real
const buscar = debounce((termino) => fetchResultados(termino), 400);

input.addEventListener('input', (e) => buscar(e.target.value));
// → fetchResultados se llama 400 ms después de que el usuario deja de escribir
```

## Parámetros de una implementación completa

Las implementaciones de producción (Lodash `_.debounce`) aceptan un tercer argumento de opciones:

| Opción | Tipo | Defecto | Efecto |
|---|---|---|---|
| `leading` | boolean | false | Ejecuta en la primera llamada del burst (antes del silencio) |
| `trailing` | boolean | true | Ejecuta al final del silencio (comportamiento por defecto) |
| `maxWait` | number | — | Garantiza ejecución aunque el burst no cese; actúa como throttle de respaldo |

### Modos de operación

**`trailing: true` (default)** — ejecuta cuando el usuario para.

**`leading: true, trailing: false`** — ejecuta inmediatamente en la primera llamada; ignora el resto del burst hasta que haya silencio. Útil para botones donde se quiere respuesta inmediata sin repetición.

**`leading: true, trailing: true`** — ejecuta al inicio y al final del burst.

**`leading: true, trailing: true, maxWait: N`** — ejecuta al inicio, al final, y además garantiza una ejecución cada N ms si el burst continúa. Combina debounce y throttle.

```js
// Ejemplo con leading — el primer clic actúa de inmediato
const enviar = debounce(submitForm, 500, { leading: true, trailing: false });
btn.addEventListener('click', enviar);
// → submitForm() se llama en el primer clic; clics rápidos siguientes se ignoran
```

## Cancelar un debounce

La función retornada puede exponer un método `cancel()` para limpiar el timer pendiente — útil al desmontar un componente o al navegar.

```js
function debounce(fn, delay) {
  let timerId;

  function debounced(...args) {
    clearTimeout(timerId);
    timerId = setTimeout(() => fn.apply(this, args), delay);
  }

  debounced.cancel = () => clearTimeout(timerId);

  return debounced;
}

const buscar = debounce(fetchResultados, 400);

// Al desmontar el componente:
buscar.cancel();
```

## Cómo funciona por dentro

Cada llamada a la función debounced ejecuta `clearTimeout(timerId)` — si había un timer pendiente, se cancela. Luego agenda `fn` con `setTimeout`. El timer solo llega a completarse cuando no se producen llamadas nuevas en `delay` ms. El cierre sobre `timerId` garantiza que instancias distintas de `debounce()` tienen timers independientes.

## Receta: búsqueda con debounce + AbortController

```js
function crearBuscador(endpoint, delay = 400) {
  let controlador;

  const buscar = debounce(async (termino) => {
    if (controlador) controlador.abort();
    controlador = new AbortController();

    try {
      const res = await fetch(`${endpoint}?q=${termino}`, {
        signal: controlador.signal
      });
      const datos = await res.json();
      renderResultados(datos);
    } catch (err) {
      if (err.name !== 'AbortError') console.error(err);
    }
  }, delay);

  return buscar;
}

const buscar = crearBuscador('/api/buscar');
input.addEventListener('input', (e) => buscar(e.target.value));
// → cada keystroke cancela la petición anterior y agenda una nueva
```

## Librerías

**Lodash** — `_.debounce(fn, wait, options)`. Soporta `leading`, `trailing`, `maxWait` y `.cancel()` / `.flush()` (fuerza ejecución inmediata).

**use-debounce (React)** — `useDebouncedCallback(fn, delay)` y el hook `useDebounce(value, delay)` para debounce de valores reactivos.

> [!tip]
> Para validación de formularios, un delay de 300–500 ms es suficiente para no interferir con el tipeo normal. Para búsqueda de API, 400–600 ms reduce las peticiones sin percepción de lentitud.

> [!warning]
> Con `async/await` dentro de la función debounced, un burst de llamadas puede generar varias promesas en vuelo si `delay` es muy corto. Combinar con `AbortController` para cancelar peticiones previas antes de la nueva.

## Notas relacionadas

- [[02 Throttle|Throttle]]
- [[index|Debouncing y Throttling — Índice]]
