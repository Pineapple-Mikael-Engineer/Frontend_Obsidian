---
title: querySelector — Primer elemento que coincide con un selector CSS
aliases:
  - querySelector
  - Element.querySelector
tags:
  - javascript
  - api/metodo
  - dom
objeto: Element
tipo: metodo
retorna: Element | null
muta: false
asincrono: false
draft: false
---

# `querySelector`

> [!definicion]
> `element.querySelector(selector)` retorna el **primer** `Element` descendiente del elemento receptor que coincide con el selector CSS `selector`, en orden de documento (depth-first, preorder). Retorna `null` si ningún descendiente coincide. Disponible en `document` y en cualquier `Element`.

```js
const nav    = document.querySelector('nav');                    // primer <nav>
const activo = document.querySelector('.menu-item.activo');      // clase compuesta
const email  = document.querySelector('input[type="email"]');    // atributo
const link   = document.querySelector('a:not([disabled])');      // pseudoclase
// → Element o null
```

## Firma

```text
element.querySelector(selector: string): Element | null
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `selector` | `string` | Cualquier selector CSS válido |

**Retorna:** el primer `Element` coincidente, o `null`. Si el selector es inválido, lanza `DOMException: SyntaxError`.

## Selectores aceptados

`querySelector` acepta la misma sintaxis que una hoja de estilos CSS:

```js
document.querySelector('#app')                    // id
document.querySelector('.card')                   // clase
document.querySelector('div > p')                 // combinador hijo
document.querySelector('ul li:first-child')       // pseudoclase
document.querySelector('[data-id="42"]')          // atributo con valor
document.querySelector('input:invalid')           // pseudoclase de formulario
document.querySelector(':not(.oculto)')           // negación
document.querySelector('h2 ~ p')                  // hermano general
```

## Contexto de elemento

Al invocar `querySelector` sobre un `Element` (no sobre `document`), la búsqueda se restringe a los descendientes de ese elemento:

```js
const form = document.querySelector('#checkout');
const primerError = form.querySelector('.campo-error');
// → solo busca dentro de #checkout
```

### La trampa de `:scope`

Sin `:scope`, el selector se evalúa respecto al **documento**, no al elemento receptor. Esto causa sorpresas con combinadores directos:

```js
const lista = document.querySelector('ul');

// Intención: hijos directos de lista
// Problema: sin :scope selecciona cualquier <li> del documento que sea hijo de cualquier <ul>
lista.querySelector('ul > li');       // puede devolver un <li> de otra <ul>

// Correcto: :scope referencia explícitamente al elemento receptor
lista.querySelector(':scope > li');   // solo hijos directos de esta <ul>
```

`:scope` está soportado en todos los navegadores modernos. En código antiguo (pre-2015) se encontrará el patrón de añadir un `id` temporal al elemento como workaround.

## Receta: primer input inválido en un formulario

```js
function focusPrimerError(form) {
  const campo = form.querySelector('input:invalid, textarea:invalid, select:invalid');
  if (campo) {
    campo.focus();
    campo.scrollIntoView({ behavior: 'smooth', block: 'center' });
  }
}

document.querySelector('form').addEventListener('submit', function (e) {
  e.preventDefault();
  focusPrimerError(this);
});
```

## Receta: elemento más cercano por selector (delegación de eventos)

```js
// Obtener el <button> que contiene el elemento clickeado, sea cual sea el hijo clicado
document.addEventListener('click', function (e) {
  const btn = e.target.closest('button[data-action]');
  if (btn) {
    console.log(btn.dataset.action);
  }
});
```

`closest` es el complemento de `querySelector`: sube por los ancestros en lugar de bajar.

## Cómo funciona por dentro

El motor evalúa el selector CSS usando el mismo motor de coincidencia que emplea para aplicar estilos. Recorre el subárbol del receptor en orden de documento (depth-first, preorder) y detiene la búsqueda en el primer elemento que satisface el selector. El resultado no es live: `querySelector` retorna una referencia al nodo tal como está en el momento de la llamada; la referencia no cambia si el DOM muta posteriormente (aunque el nodo en sí sí refleja cambios).

La complejidad depende del selector y del tamaño del subárbol. Es más lenta que `getElementById` (que usa tabla hash O(1)) pero ofrece expresividad total de CSS.

> [!tip] Buenas prácticas
> - Preferir `querySelector` sobre `getElementById` cuando el selector puede cambiar o cuando la legibilidad importa más que los microsegundos.
> - Acotar el contexto al subárbol más pequeño posible (`form.querySelector(...)` en lugar de `document.querySelector(...)`) para mejorar rendimiento y claridad.
> - Usar `:scope` explícitamente cuando se trabaja con combinadores directos sobre un elemento contexto.
> - Validar con `if (el)` antes de acceder a propiedades: `querySelector` puede retornar `null`.

> [!warning] Errores comunes
> - Selector CSS inválido (comillas sin cerrar, combinadores sin operando): lanza `DOMException: SyntaxError` en tiempo de ejecución. El error no se detecta en tiempo de análisis.
> - Asumir que busca solo dentro del elemento contexto sin `:scope`: combinadores como `> .hijo` se evalúan respecto al documento.
> - Confundir "primer elemento en el DOM" con "primer elemento visualmente": el orden es el de aparición en el árbol del documento, no el visual (que puede cambiar con CSS).
> - Olvidar que `querySelector` retorna `null` (no `undefined`) cuando no hay coincidencias.

## Notas relacionadas

- [[index | Selección de Elementos]]
- [[04 querySelectorAll]]
- [[01 getElementById]]
- [[05 HTMLCollection vs NodeList]]
