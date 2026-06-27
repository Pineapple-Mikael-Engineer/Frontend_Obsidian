---
title: target vs currentTarget — Origen e interceptor del evento
aliases:
  - e.target
  - e.currentTarget
  - target currentTarget
tags:
  - javascript
  - api/concepto
  - eventos
draft: false
order: 1
---

# target vs currentTarget

> [!definicion]
> `e.target` es el elemento que **originó** el evento — donde ocurrió la interacción del usuario. `e.currentTarget` es el elemento donde está **registrado el listener que se está ejecutando ahora**. Son iguales cuando el listener está en el mismo elemento que disparó el evento; difieren cuando el evento ha burbujeado hasta un ancestro.

```js
// HTML: <ul id="lista"><li>Elemento</li></ul>

const lista = document.querySelector('#lista');

lista.addEventListener('click', (e) => {
  console.log(e.target);         // el <li> clicado
  console.log(e.currentTarget);  // el <ul> (donde está el listener)
});
```

## Cuándo son distintos

En burbujeo: el evento asciende desde el `<li>` hasta el `<ul>`. El handler en `<ul>` ve `e.target = <li>` y `e.currentTarget = <ul>`.

```
HTML:
<ul id="lista">
  <li class="item">Primero</li>
  <li class="item">Segundo</li>
</ul>

Click en "Primero":
  e.target        → <li class="item">Primero</li>
  e.currentTarget → <ul id="lista">
```

En delegación de eventos, el listener vive en el ancestro (`<ul>`) y `e.target` identifica cuál de los hijos fue clicado.

## `currentTarget` fuera del handler

El spec establece que `e.currentTarget` se pone a `null` una vez que el despacho del evento termina. Almacenar `e` en una variable y acceder a `currentTarget` después del handler retorna `null`.

```js
let eventoGuardado;

lista.addEventListener('click', (e) => {
  eventoGuardado = e;
  console.log(e.currentTarget);  // → <ul> (correcto dentro del handler)
});

// Más tarde, fuera del handler:
console.log(eventoGuardado.currentTarget);  // → null
console.log(eventoGuardado.target);         // → <li> (sigue disponible)
```

## Delegación con `closest`

El patrón estándar de delegación: listener en el contenedor, `closest` para encontrar el descendiente relevante aunque el clic sea en un hijo interior del item.

```js
lista.addEventListener('click', (e) => {
  // e.target puede ser un <span> o <img> dentro del .item
  const item = e.target.closest('.item');
  if (!item || !lista.contains(item)) return;

  // actuar sobre item
  item.classList.toggle('seleccionado');
});
```

`lista.contains(item)` protege contra el caso en que `closest` suba hasta un `.item` fuera del contenedor (si el selector existe en un ancestro lejano).

> [!tip]
> Para saber en qué elemento está el listener activo, usar siempre `e.currentTarget` — no `this`, que puede diferir si el handler es una arrow function. Para saber qué elemento interactuó el usuario, usar `e.target`.

> [!warning]
> En componentes con HTML anidado, el `e.target` puede ser un hijo muy profundo del elemento visual que le interesa al handler (p. ej. un `<span>` dentro de un `<button>`). Siempre usar `e.target.closest('.selector')` en lugar de comparar `e.target` directamente con un elemento específico.

## Notas relacionadas

- [[05 Delegación de Eventos|Delegación de Eventos]]
- [[04 Fases del Evento/index|Fases del Evento]]
- [[03 El Objeto Event/index|El Objeto Event]]
