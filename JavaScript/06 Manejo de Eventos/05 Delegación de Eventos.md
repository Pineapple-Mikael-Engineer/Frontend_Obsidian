---
title: Delegación de Eventos — Un listener para gobernarlos a todos
aliases:
  - delegación de eventos
  - event delegation
tags:
  - javascript
  - api/concepto
  - eventos
draft: false
order: 1
---

# Delegación de Eventos

> [!definicion]
> La **delegación de eventos** consiste en registrar un único listener en un elemento ancestro en lugar de listeners individuales en cada elemento hijo. Funciona gracias al [[04 Fases del Evento/index|burbujeo]]: el evento que ocurre en un hijo asciende hasta el ancestro, donde el listener inspecciona `e.target` para determinar en qué descendiente se originó y actuar en consecuencia.

```js
const lista = document.querySelector('#lista');

// Un solo listener gestiona todos los <li>, presentes y futuros
lista.addEventListener('click', (e) => {
  const item = e.target.closest('.item');
  if (!item || !lista.contains(item)) return;

  item.classList.toggle('seleccionado');
});
```

## Por qué funciona

Cuando el usuario hace clic en un `<li>`, el evento burbujea: `<li>` → `<ul>` → `<body>` → … Al llegar al `<ul>`, el listener se ejecuta y `e.target` apunta al `<li>` original. El ancestro actúa como intermediario centralizado.

## Ventajas

**Eficiencia de memoria:** un listener en el contenedor vs N listeners (uno por hijo). En listas de 1000 elementos, la diferencia es significativa.

**Elementos dinámicos:** los hijos añadidos al DOM después de registrar el listener también disparan el evento hacia el ancestro. No es necesario registrar un nuevo listener al insertar elementos.

```js
// El listener ya existe antes de añadir nuevos items
function añadirItem(texto) {
  const li = document.createElement('li');
  li.className = 'item';
  li.textContent = texto;
  lista.appendChild(li);
  // → el clic en este li ya es gestionado automáticamente
}
```

## El patrón correcto con `closest` y `contains`

```js
contenedor.addEventListener('click', (e) => {
  // closest sube por el árbol hasta encontrar .item (o null)
  const item = e.target.closest('.item');

  // contains protege: si .item existe fuera del contenedor,
  // closest lo encontraría igualmente — verificar que está dentro
  if (!item || !contenedor.contains(item)) return;

  procesarItem(item);
});
```

`e.target.closest('.selector')` es robusto ante HTML anidado: si el `<li>` contiene un `<span>`, `<img>` u otro hijo, el clic puede recaer en el hijo interior. `closest` sube desde ese hijo hasta el `.item` más cercano.

## Delegación con `data-*` para múltiples acciones

```js
// HTML: <button data-accion="editar" data-id="5">Editar</button>
//       <button data-accion="eliminar" data-id="5">Eliminar</button>

tabla.addEventListener('click', (e) => {
  const btn = e.target.closest('[data-accion]');
  if (!btn || !tabla.contains(btn)) return;

  const { accion, id } = btn.dataset;

  if (accion === 'editar') abrirEditor(id);
  if (accion === 'eliminar') confirmarEliminar(id);
});
```

## Tabla con botones de acción por fila — receta completa

```js
const tbody = document.querySelector('#tabla tbody');

tbody.addEventListener('click', (e) => {
  const btn = e.target.closest('button[data-accion]');
  if (!btn || !tbody.contains(btn)) return;

  const fila = btn.closest('tr');
  const id = fila.dataset.id;

  switch (btn.dataset.accion) {
    case 'ver':     verDetalle(id);      break;
    case 'editar':  abrirFormulario(id); break;
    case 'borrar':  eliminarFila(fila);  break;
  }
});
```

## Cuándo NO delegar

**Eventos que no burbujean:** `focus`, `blur`, `mouseenter`, `mouseleave`. Usar `focusin`/`focusout` y `mouseover`/`mouseout` en su lugar, que sí burbujean.

**Hijos que llaman `stopPropagation`:** si algún hijo detiene la propagación, el evento nunca alcanza el ancestro delegado y el listener no se ejecuta.

**Listeners con lógica muy específica por elemento:** si cada hijo necesita un closure con estado propio, delegar añade complejidad sin beneficio claro.

> [!tip]
> Nombrar el selector en una constante al inicio del handler (`const item = e.target.closest('.item')`) hace el patrón más legible y evita repetir la misma llamada.

> [!warning]
> `closest` no solo busca dentro del contenedor — sube por todo el árbol DOM hasta `<html>`. Sin la guarda `contenedor.contains(item)`, un `.item` que exista fuera del contenedor (más arriba en el árbol) podría disparar la lógica incorrectamente.

## Notas relacionadas

- [[04 Fases del Evento/index|Fases del Evento]]
- [[03 Burbujeo|Burbujeo]]
- [[01 target vs currentTarget|target vs currentTarget]]
- [[03 stopPropagation y stopImmediatePropagation|stopPropagation]]
