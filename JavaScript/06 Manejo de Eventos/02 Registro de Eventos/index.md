---
title: Registro de Eventos — Índice
aliases:
  - Registrar Eventos
  - Event Registration
tags:
  - javascript
  - api/concepto
  - eventos
draft: false
---

# Registro de Eventos

> [!definicion]
> Registrar un evento es vincular una función (handler o listener) a un elemento del DOM para que el motor la ejecute cuando ocurra un evento de un tipo determinado. Existen tres mecanismos: el atributo HTML `on*`, la propiedad JS `onclick`/`on*`, y el método `addEventListener`. Solo el tercero permite múltiples handlers, control de fase y opciones avanzadas.

El flujo mínimo:

```js
const btn = document.querySelector('#enviar');

btn.addEventListener('click', (e) => {
  console.log('clic en', e.target);
});
```

## Comparativa de mecanismos

| Mecanismo | Sintaxis | Múltiples handlers | Opciones (capture, once, passive) | Separación HTML/JS |
|---|---|---|---|---|
| Atributo HTML `on*` | `<button onclick="fn()">` | No | No | No |
| Propiedad JS `on*` | `el.onclick = fn` | No (el último sobreescribe) | No | Sí |
| `addEventListener` | `el.addEventListener('click', fn)` | Sí | Sí | Sí |

La forma recomendada es siempre `addEventListener`: no impone límite de un handler por evento, permite controlar la fase (captura o burbujeo), acepta opciones como `once` y `passive`, y puede eliminarse con `removeEventListener` o con un `AbortSignal`.

## Subsecciones

| # | Nota | Cubre |
|---|---|---|
| 01 | addEventListener | Firma completa, handler función u objeto, comportamiento de `this` |
| 02 | Opciones (once, passive, capture) | Tercer argumento, tabla de opciones, `AbortController` |
| 03 | removeEventListener | Requisito de referencia, coincidencia de fase, alternativa con `AbortController` |
| 04 | Métodos Antiguos (onclick) | Atributo HTML, propiedad JS, por qué evitarlos |

## Notas relacionadas

- [[01 addEventListener|addEventListener]]
- [[02 Opciones (once, passive, capture)|Opciones (once, passive, capture)]]
- [[03 removeEventListener|removeEventListener]]
- [[04 Métodos Antiguos (onclick)|Métodos Antiguos (onclick)]]
- [[03 El Objeto Event/index|El Objeto Event]]
