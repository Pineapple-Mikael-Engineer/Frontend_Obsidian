---
title: childNodes y children — Colecciones de hijos
aliases:
  - childNodes
  - children
  - firstElementChild
  - lastElementChild
  - childElementCount
tags:
  - javascript
  - api/metodo
  - dom
objeto: Node
tipo: metodo
retorna: NodeList | HTMLCollection
muta: false
asincrono: false
draft: false
---

# `childNodes` y `children`

> [!definicion]
> `nodo.childNodes` devuelve una `NodeList` **live** con todos los hijos del nodo: Elements, nodos de texto (incluyendo espacios y saltos de línea) y comentarios. `elemento.children` devuelve una `HTMLCollection` **live** con solo los hijos que son `Element`. En HTML real formateado con indentación, `childNodes` casi siempre incluye nodos de texto invisibles entre los elementos.

```js
const lista = document.querySelector('ul');

// Todos los hijos, incluyendo nodos de texto
lista.childNodes;       // NodeList [text, li, text, li, text, li, text]

// Solo los <li> Elements
lista.children;         // HTMLCollection [li, li, li]
lista.children.length;  // → 3

// Primero y último Element
lista.firstElementChild;  // → primer <li>
lista.lastElementChild;   // → último <li>

// Cantidad de hijos Element
lista.childElementCount;  // → 3
```

## Tabla comparativa

| Propiedad | Interfaz | Tipo devuelto | Live | Incluye nodos de texto | `forEach` directo |
|---|---|---|---|---|---|
| `childNodes` | `Node` | `NodeList` | sí | sí | sí |
| `children` | `Element` | `HTMLCollection` | sí | no | no |

> [!warning] `HTMLCollection` no tiene `forEach`
> `children` devuelve `HTMLCollection`, que no implementa `forEach` directamente. Para iterar: `[...elemento.children].forEach(...)` o `Array.from(elemento.children).forEach(...)`.

## La trampa del espacio en blanco

```html
<ul>
  <li>Primero</li>
  <li>Segundo</li>
</ul>
```

```js
const ul = document.querySelector('ul');

ul.childNodes[0];   // → Text "\n  "   ← nodo de texto con la indentación
ul.childNodes[1];   // → <li>Primero</li>
ul.childNodes[2];   // → Text "\n  "
ul.childNodes[3];   // → <li>Segundo</li>
ul.childNodes[4];   // → Text "\n"

ul.children[0];     // → <li>Primero</li>  ← sin sorpresas
ul.children[1];     // → <li>Segundo</li>
```

El HTML minificado (sin espacios entre etiquetas) no tiene nodos de texto intermedios, pero en código de producción con indentación o con HTML servido por el servidor, `childNodes[0]` es casi siempre un nodo de texto.

## Propiedades de primer y último hijo

```js
const ul = document.querySelector('ul');

// Con nodos de texto incluidos
ul.firstChild;  // → Text "\n  "  (nodo de texto)
ul.lastChild;   // → Text "\n"    (nodo de texto)

// Solo Elements — lo que normalmente se necesita
ul.firstElementChild;  // → <li>Primero</li>
ul.lastElementChild;   // → <li>Segundo</li>
```

## Receta: iterar solo elementos hijos

```js
const menu = document.querySelector('nav ul');

// Spread + for...of — legible y sin problemas de live collection
for (const item of [...menu.children]) {
  item.classList.toggle('activo', item.dataset.id === idActual);
}

// Alternativa con Array.from
Array.from(menu.children).forEach(item => {
  // ...
});
```

Evitar iterar directamente sobre una `HTMLCollection` con índice mientras se modifica el DOM: al ser live, añadir o quitar elementos altera `length` durante el bucle.

## Receta: comprobar si un elemento tiene hijos Element

```js
const contenedor = document.querySelector('.wrapper');

if (contenedor.childElementCount === 0) {
  contenedor.textContent = 'Sin resultados';
}
```

`childElementCount` es más directo que `contenedor.children.length` y expresa mejor la intención.

## Cómo funciona por dentro

> [!info]
> Ambas propiedades devuelven colecciones **live**: no son snapshots, sino vistas directas sobre la lista interna de hijos del nodo. Cada vez que se accede a `length` o a un índice, el motor consulta el estado actual del árbol. Esto tiene consecuencias: modificar el DOM mientras se itera con índice puede producir saltos o repeticiones de elementos.
>
> `childNodes` es un objeto `NodeList` que expone todos los nodos hijos sin filtro. `children` es una `HTMLCollection` que internamente filtra solo los nodos con `nodeType === 1` (Element). Ambas reflejan cambios inmediatamente porque apuntan a la misma estructura interna del árbol.

> [!tip] Buenas prácticas
> - Preferir `children`, `firstElementChild`, `lastElementChild` y `childElementCount` en código de producto: evitan el ruido de nodos de texto.
> - Convertir a array antes de iterar si se va a mutar el DOM durante el bucle: `[...elemento.children]` produce una copia estática.
> - Usar `childNodes` solo cuando se necesita acceder deliberadamente a nodos de texto o comentarios.

> [!warning] Errores comunes
> - Acceder a `childNodes[0]` esperando el primer elemento visible: casi siempre es un nodo de texto en HTML formateado. Usar `firstElementChild`.
> - Llamar `children.forEach(...)` directamente: `HTMLCollection` no implementa `forEach`; lanza `TypeError`. Necesita conversión a array.
> - Iterar `childNodes` con `for (let i = 0; i < node.childNodes.length; i++)` y eliminar hijos en cada iteración: al ser live, `length` decrece y se saltan nodos. Iterar al revés o convertir primero a array.

## Notas relacionadas

- [[index | Recorrer el DOM]]
- [[03 Hermanos (nextSibling, previousSibling)]]
- [[01 Selección de Elementos/05 HTMLCollection vs NodeList | HTMLCollection vs NodeList]]
