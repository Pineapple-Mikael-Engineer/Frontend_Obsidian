---
title: AbortController — cancelar peticiones fetch
aliases:
  - AbortController
  - AbortSignal
  - abort fetch
  - cancelar fetch
tags:
  - javascript
  - api/clase
  - red
objeto: AbortController
tipo: clase
retorna: AbortController
muta: false
asincrono: false
draft: false
---

# `AbortController`

> [!definicion]
> `AbortController` es una clase del navegador que permite cancelar peticiones `fetch` (y otras operaciones basadas en señales) en curso. Se crea un controlador, su `signal` se pasa a `fetch`, y al llamar a `controller.abort()` la petición rechaza con un `AbortError`. Un mismo `signal` puede pasarse a múltiples peticiones y cancelarlas todas simultáneamente.

```js
const controller = new AbortController();

try {
  const res = await fetch('/api/datos', { signal: controller.signal });
  const data = await res.json();
} catch (err) {
  if (err.name === 'AbortError') return; // cancelación intencional, no error
  throw err;
}

// En otro punto del código (ej. usuario cancela, componente desmonta):
controller.abort();
```

## API de `AbortController`

| Miembro | Tipo | Descripción |
|---|---|---|
| `controller.signal` | `AbortSignal` | El signal a pasar a fetch u otras APIs |
| `controller.abort(razon?)` | `void` | Cancela inmediatamente; `razon` es el valor de `signal.reason` |
| `controller.signal.aborted` | `boolean` | `true` si ya se abortó |
| `controller.signal.reason` | `any` | La razón pasada a `abort()` |

## `AbortSignal.timeout(ms)` — timeout sin `setTimeout`

Crea un `AbortSignal` que se aborta automáticamente después de `ms` milisegundos. No requiere gestionar un `setTimeout` ni limpiarlo manualmente.

```js
// Cancelar si el servidor no responde en 5 segundos
const res = await fetch('/api/datos', {
  signal: AbortSignal.timeout(5000),
});
```

Al agotar el tiempo, la Promise rechaza con `TimeoutError` (no `AbortError`): `err.name === 'TimeoutError'`.

## `AbortSignal.any([signal1, signal2])` — combinación de señales

Se aborta cuando cualquiera de los signals recibidos se aborta. Permite combinar un timeout con una cancelación manual.

```js
const controller = new AbortController();
const signal = AbortSignal.any([
  controller.signal,
  AbortSignal.timeout(10_000),
]);

const res = await fetch('/api/lento', { signal });

// Se cancela si el usuario pulsa "cancelar" O si pasan 10 segundos
cancelarBtn.onclick = () => controller.abort('usuario canceló');
```

## Cancelar búsqueda al escribir (debounce + abort)

```js
let controllerBusqueda = null;

inputBusqueda.addEventListener('input', async (e) => {
  // Cancelar la petición anterior si aún está en curso
  if (controllerBusqueda) controllerBusqueda.abort();

  controllerBusqueda = new AbortController();
  const query = e.target.value.trim();

  if (!query) return;

  try {
    const res = await fetch(`/api/buscar?q=${encodeURIComponent(query)}`, {
      signal: controllerBusqueda.signal,
    });
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    const resultados = await res.json();
    mostrarResultados(resultados);
  } catch (err) {
    if (err.name === 'AbortError') return; // petición anterior cancelada — normal
    console.error('Error en búsqueda:', err.message);
  }
});
```

Este patrón garantiza que solo la última petición iniciada puede mostrar resultados — las anteriores se cancelan antes de que puedan llegar a `mostrarResultados`.

## Cleanup en React `useEffect`

```js
import { useEffect, useState } from 'react';

function PerfilUsuario({ id }) {
  const [usuario, setUsuario] = useState(null);

  useEffect(() => {
    const controller = new AbortController();

    async function cargar() {
      try {
        const res = await fetch(`/api/usuarios/${id}`, {
          signal: controller.signal,
        });
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        const data = await res.json();
        setUsuario(data);
      } catch (err) {
        if (err.name === 'AbortError') return; // componente desmontado — normal
        console.error('Error al cargar usuario:', err.message);
      }
    }

    cargar();

    // Cleanup: se ejecuta al desmontar o al cambiar `id`
    return () => controller.abort();
  }, [id]);

  return usuario ? <div>{usuario.nombre}</div> : <p>Cargando…</p>;
}
```

El cleanup de `useEffect` garantiza que si el componente se desmonta mientras la petición está en curso (navegación rápida, cambio de ruta), la respuesta no intenta actualizar el estado de un componente desmontado.

## Un signal para múltiples peticiones

```js
const controller = new AbortController();
const { signal } = controller;

// Las tres peticiones se cancelan con un solo abort()
const [usuarios, productos, pedidos] = await Promise.all([
  fetch('/api/usuarios', { signal }).then(r => r.json()),
  fetch('/api/productos', { signal }).then(r => r.json()),
  fetch('/api/pedidos', { signal }).then(r => r.json()),
]);

// Si el usuario navega antes de que lleguen:
controller.abort();
```

## Cómo funciona por dentro

`AbortController` y `AbortSignal` implementan el patrón Observer del estándar DOM. Al pasar `signal` a `fetch`, el motor de red registra un listener en el signal. Cuando se llama a `abort()`, el signal dispara un evento `'abort'`; el motor de red recibe ese evento, cancela la petición HTTP subyacente (o descarta la respuesta si ya llegó) y rechaza la Promise con `AbortError`. El comportamiento está especificado en el estándar Fetch de WHATWG.

> [!tip]
> Crear un nuevo `AbortController` por efecto/operación, no reutilizar el mismo para múltiples operaciones independientes. Una vez que un controlador se aborta, su `signal.aborted` queda `true` de forma permanente y cualquier nuevo `fetch` que reciba ese signal rechazará inmediatamente.

> [!warning]
> No olvidar el check `if (err.name === 'AbortError') return` en el catch. Sin él, la cancelación intencional se trata como un error y puede disparar logging de errores, alertas al usuario o lógica de retry — todo innecesariamente. En React, también evita el warning de "update on unmounted component".

## Notas relacionadas

- [[06 Manejo de Errores|Manejo de Errores]] — AbortError en la estrategia general de errores
- [[01 Sintaxis Básica|Sintaxis Básica]] — la opción `signal` en el objeto de configuración de fetch
- [[03 mode, credentials, cache|mode, credentials, cache]] — otras opciones del RequestInit
