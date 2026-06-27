---
title: Métodos Antiguos de Registro de Eventos — onclick y atributos on*
aliases:
  - onclick
  - atributos on*
  - métodos legacy de eventos
tags:
  - javascript
  - api/concepto
  - eventos
draft: false
order: 4
---

# Métodos Antiguos (onclick)

> [!definicion]
> Antes de `addEventListener`, los eventos se registraban de dos formas: atributos HTML `on*` (inline en el markup) y propiedades JS `on*` (asignadas desde script). Ambas solo permiten un handler por evento y por elemento, no aceptan opciones de fase ni `passive`, y el último en asignarse sobreescribe al anterior.

## Atributo HTML `on*`

El atributo ejecuta el string como JS con el elemento como contexto:

```html
<button onclick="procesarPedido()">Enviar</button>
<input oninput="validar(this.value)">
<form onsubmit="return false">...</form>
```

El string se evalúa con `eval`-like semántica. `this` dentro del string es el elemento. No hay referencia al objeto `Event` a menos que se pase explícitamente con `event`:

```html
<button onclick="handler(event)">Clic</button>
```

## Propiedad JS `on*`

Asignar directamente a la propiedad del elemento es más limpio que el atributo HTML, pero sigue limitado a un handler por evento:

```js
const btn = document.querySelector('#btn');

btn.onclick = function(e) {
  console.log('primer handler', e.target);
};

// El segundo sobreescribe al primero
btn.onclick = function(e) {
  console.log('segundo handler — el primero ya no existe');
};

// Eliminar
btn.onclick = null;
```

El `onclick` del elemento y el atributo HTML `onclick` son la misma propiedad: asignar uno sobreescribe el otro.

## Propiedades `on*` en `window`

Algunos handlers globales siguen siendo habituales en código existente:

| Propiedad | Equivalente con addEventListener | Cuándo se dispara |
|---|---|---|
| `window.onload` | `window.addEventListener('load', fn)` | Todo el recurso cargado (imgs, scripts) |
| `window.onerror` | `window.addEventListener('error', fn)` | Error JS no capturado |
| `window.onunload` | `window.addEventListener('unload', fn)` | Antes de descargar la página |
| `window.onresize` | `window.addEventListener('resize', fn)` | Al redimensionar la ventana |

## Por qué no usar estos métodos

| Limitación | Atributo HTML | Propiedad JS |
|---|---|---|
| Solo un handler por evento | Sí | Sí |
| No acepta opciones (capture, passive, once) | Sí | Sí |
| Mezcla JS en el HTML | Sí | No |
| Difícil de mantener en apps grandes | Sí | Sí |
| No funciona con `removeEventListener` | Sí | No aplica |

> [!warning]
> Frameworks y librerías de terceros también usan `addEventListener` internamente. Si se asigna `el.onclick = fn` sobre un elemento que ya tenía un listener de otro sistema, se sobreescribe sin warning. El código resultante es impredecible.

## Cuándo los verás

- Ejemplos de tutorial simplificados o de hace más de 10 años.
- Código legacy de aplicaciones sin módulos (jQuery pre-1.7, ASP.NET Web Forms).
- Snippets rápidos de prueba en la consola del navegador.
- Generadores de código de plataformas antiguas (Google Tag Manager clásico, algunos CMS).

> [!tip]
> Al mantener código legacy, migrar `el.onclick = fn` a `el.addEventListener('click', fn)` es seguro y no cambia el comportamiento siempre que no hubiese ya otros listeners en el mismo elemento — en cuyo caso el cambio puede activar handlers que antes estaban silenciados.

## Notas relacionadas

- [[01 addEventListener|addEventListener]]
- [[02 Registro de Eventos/index|Registro de Eventos]]
