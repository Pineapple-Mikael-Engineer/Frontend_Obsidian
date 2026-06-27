---
title: stopPropagation y stopImmediatePropagation — Detener la propagación de eventos
aliases:
  - stopPropagation
  - stopImmediatePropagation
  - e.stopPropagation
tags:
  - javascript
  - api/metodo
  - eventos
objeto: Event
tipo: metodo
retorna: undefined
muta: false
asincrono: false
draft: false
order: 3
---

# stopPropagation y stopImmediatePropagation

> [!definicion]
> `e.stopPropagation()` detiene el viaje del evento por el árbol DOM — en burbujeo, el evento no asciende a los ancestros; en captura, no desciende más. Los demás listeners registrados en el **mismo elemento** para el mismo evento siguen ejecutándose. `e.stopImmediatePropagation()` hace lo mismo y además cancela todos los listeners pendientes del elemento actual para este evento.

```js
const modal = document.querySelector('.modal');
const overlay = document.querySelector('.overlay');

// El clic dentro del modal no debe cerrar el modal
modal.addEventListener('click', (e) => {
  e.stopPropagation();  // el evento no llega al overlay
});

overlay.addEventListener('click', () => {
  cerrarModal();
});
```

## Comparativa

| Método | Detiene propagación a ancestros/descendientes | Cancela otros listeners del mismo elemento |
|---|---|---|
| `stopPropagation()` | Sí | No |
| `stopImmediatePropagation()` | Sí | Sí |

```js
btn.addEventListener('click', (e) => {
  console.log('listener 1');
  e.stopPropagation();        // el evento no sube
  // listener 2 del mismo btn sí se ejecuta
});

btn.addEventListener('click', (e) => {
  console.log('listener 2'); // se ejecuta igualmente
});

// Con stopImmediatePropagation en listener 1:
// → listener 2 NO se ejecuta
```

## Cuándo usar `stopPropagation`

**Modal que no cierra al clicar su interior:** el overlay tiene un listener de cierre; el interior del modal llama `stopPropagation` para que el clic no suba hasta el overlay.

**Componente encapsulado:** un widget que no debe dejar escapar sus eventos internos hacia el documento padre — evita interferir con handlers de la aplicación que escuchan en `document`.

**Tooltips y dropdowns:** el documento tiene un listener global de cierre (`document.addEventListener('click', cerrarDropdown)`); el dropdown llama `stopPropagation` en sus propios clics internos.

## Cuándo usar `stopImmediatePropagation`

Cuando varios handlers independientes están en el mismo elemento y uno de ellos debe tener prioridad exclusiva para ese evento concreto. Raro en práctica ordinaria; más habitual en sistemas de plugins donde un plugin puede vetar el resto.

## Antipatrón: uso indiscriminado

> [!warning]
> Llamar `stopPropagation` de forma rutinaria rompe la [[05 Delegación de Eventos|delegación de eventos]] y los listeners globales (analíticas, accesibilidad, sistemas de logging). Si un componente hijo llama `stopPropagation` en todos sus clics, el ancestro que delega nunca recibirá el evento. El resultado es código funcionalmente correcto en aislamiento pero roto al integrarse con el resto de la aplicación.

Alternativa: en lugar de `stopPropagation`, comprobar en el handler del ancestro si el evento proviene de una zona que debe ignorarse:

```js
// En lugar de stopPropagation en el modal:
overlay.addEventListener('click', (e) => {
  if (modal.contains(e.target)) return;  // ignorar clics en el modal
  cerrarModal();
});
```

> [!tip]
> `stopPropagation` y `preventDefault` son ortogonales. `stopPropagation` controla si el evento viaja por el árbol; `preventDefault` controla si el navegador ejecuta su acción por defecto. Se pueden combinar o usar por separado según el caso.

## Notas relacionadas

- [[02 preventDefault|preventDefault]]
- [[04 Fases del Evento/index|Fases del Evento]]
- [[05 Delegación de Eventos|Delegación de Eventos]]
- [[03 El Objeto Event/index|El Objeto Event]]
