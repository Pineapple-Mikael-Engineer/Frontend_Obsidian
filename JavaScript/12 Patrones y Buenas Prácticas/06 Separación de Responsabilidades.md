---
title: Separación de Responsabilidades
aliases:
  - SoC
  - Separation of Concerns
  - SRP javascript
tags:
  - javascript
  - api/concepto
  - patrones
draft: false
order: 3
---

# Separación de Responsabilidades

> [!definicion]
> El **principio de separación de responsabilidades** (SoC — *Separation of Concerns*) dicta que cada módulo, función o clase debe tener **una única responsabilidad bien definida** y no mezclar capas de abstracción distintas. En frontend JS, se traduce en separar la lógica de negocio del DOM, los datos de la presentación, y la configuración de la lógica operativa.

## Capas en una aplicación frontend

| Capa | Responsabilidad | Ejemplo en JS |
|---|---|---|
| Datos / estado | Almacenar y actualizar el estado de la app | Objetos planos, arrays, clases de modelo |
| Lógica de negocio | Calcular, transformar, validar datos | Funciones puras sin acceso al DOM |
| Presentación / UI | Leer el estado y actualizar el DOM | Funciones que llaman `querySelector`, `textContent` |
| Configuración | URLs de API, timeouts, textos de error | Archivos `config.js` con constantes exportadas |
| Comunicación | Peticiones a red, WebSockets | Módulos de servicio con `fetch` / `WebSocket` |

## Lógica de negocio vs manipulación del DOM

```js
// Mal — lógica de negocio mezclada con DOM
function procesarPedido(items) {
  let total = 0;
  items.forEach(item => {
    total += item.precio * item.cantidad;
    document.querySelector(`#item-${item.id}`).classList.add("procesado");
  });
  document.querySelector("#total").textContent = `$${total}`;
}

// Bien — separado en dos responsabilidades
function calcularTotal(items) {
  return items.reduce((acc, item) => acc + item.precio * item.cantidad, 0);
}

function renderPedido(items, total) {
  items.forEach(item => {
    document.querySelector(`#item-${item.id}`).classList.add("procesado");
  });
  document.querySelector("#total").textContent = `$${total}`;
}

// Coordinación en el event handler
boton.addEventListener("click", () => {
  const total = calcularTotal(carrito.items);
  renderPedido(carrito.items, total);
});
```

`calcularTotal` se puede testear sin el DOM. `renderPedido` se puede reemplazar sin tocar la lógica.

## Separar configuración de lógica

```js
// config.js
export const API = {
  BASE_URL: "https://api.ejemplo.com",
  TIMEOUT_MS: 5000,
  MAX_REINTENTOS: 3,
};

export const MENSAJES = {
  ERROR_RED: "No se pudo conectar. Intenta más tarde.",
  ERROR_AUTENTICACION: "Sesión expirada. Inicia sesión de nuevo.",
};

// servicio.js — importa config, no hardcodea
import { API, MENSAJES } from "./config.js";

async function obtenerUsuario(id) {
  const res = await fetch(`${API.BASE_URL}/usuarios/${id}`, {
    signal: AbortSignal.timeout(API.TIMEOUT_MS),
  });
  if (!res.ok) throw new Error(MENSAJES.ERROR_RED);
  return res.json();
}
```

Cambiar el entorno (dev → prod) requiere editar solo `config.js`, no buscar valores hardcodeados en todos los módulos.

## Señales de violación de SoC

- Funciones de más de 50-80 líneas que realizan operaciones de naturalezas distintas.
- Nombres que revelan múltiples responsabilidades: `procesarYMostrarYGuardar`, `validarYRenderizar`.
- Lógica de negocio dentro de event handlers directamente (en lugar de delegarla a funciones puras).
- Manipulación del DOM dentro de funciones de cálculo o validación.
- Constantes de configuración dispersas como literales en distintos archivos.
- Tests que requieren simular el DOM para verificar una regla de negocio.

## Beneficios

**Testabilidad**: las funciones puras de lógica se testean sin simular el DOM ni hacer peticiones de red. La cobertura es más rápida y fiable.

**Reutilización**: `calcularTotal` puede usarse en un contexto de servidor, en un test, en una vista distinta — sin arrastrar dependencias del DOM.

**Legibilidad**: cada función hace exactamente lo que su nombre indica. El lector no necesita rastrear efectos secundarios ocultos.

**Mantenibilidad**: cambiar la librería de UI, migrar a un framework, o cambiar un endpoint no requiere reescribir la lógica de negocio.

## Cómo funciona por dentro

SoC no es una regla del lenguaje: es una decisión de diseño que el motor JS no impone. Se implementa como convención: separar archivos por capa, no por funcionalidad. En lugar de `usuario.js` (que mezcla fetch, validación y render), se tiene `servicios/usuario.js` (fetch), `logica/validarUsuario.js` (reglas) y `ui/renderUsuario.js` (DOM). Las dependencias fluyen en una dirección: UI → lógica → servicios → config, nunca al revés.

> [!tip]
> El test de separación rápido: si para testear una función de cálculo o validación necesitas un `document` o un `fetch` real, SoC se está violando. Las funciones de lógica de negocio no deben saber nada de su entorno de ejecución.

> [!warning]
> La separación excesiva — una función de dos líneas en su propio archivo — introduce overhead de imports sin beneficio real. SoC se aplica a nivel de responsabilidades semánticamente distintas (red, DOM, lógica), no a nivel de cada pequeña operación. La granularidad correcta depende del tamaño y vida esperada del proyecto.

## Notas relacionadas

- [[07 Código Limpio|Código Limpio]]
- [[04 Memoización|Memoización]]
- [[05 Patrones de Diseño/index|Patrones de Diseño]]
