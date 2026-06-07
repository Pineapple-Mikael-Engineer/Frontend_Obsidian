---
title: Hermanos — nextSibling, previousSibling y variantes Element
aliases:
  - nextSibling
  - previousSibling
  - nextElementSibling
  - previousElementSibling
  - hermanos DOM
tags:
  - javascript
  - api/metodo
  - dom
objeto: Node
tipo: metodo
retorna: Node | Element | null
muta: false
asincrono: false
draft: false
---

# Hermanos: `nextSibling` y `previousSibling`

> [!definicion]
> `nodo.nextSibling` y `nodo.previousSibling` devuelven el nodo hermano inmediatamente siguiente o anterior en la lista de hijos del padre; incluyen nodos de texto y comentarios. `elemento.nextElementSibling` y `elemento.previousElementSibling` hacen lo mismo pero devuelven solo el hermano que sea `Element`, saltando nodos de texto. Todos devuelven `null` si no existe el hermano pedido.

```js
const item = document.querySelector('li.activo');

// Incluye nodos de texto — frecuentemente devuelve Text "\n"
item.nextSibling;       // → Text "\n  " en HTML formateado
item.previousSibling;   // → Text "\n  "

// Solo Elements — lo habitual en código de producto
item.nextElementSibling;     // → el <li> siguiente, o null si es el último
item.previousElementSibling; // → el <li> anterior, o null si es el primero
```

## Firma

```text
Node.nextSibling: Node | null
Node.previousSibling: Node | null
Element.nextElementSibling: Element | null
Element.previousElementSibling: Element | null
```

Todas son propiedades de solo lectura; O(1) por ser punteros directos.

## La trampa del espacio en blanco

```html
<ul>
  <li id="a">A</li>
  <li id="b">B</li>
  <li id="c">C</li>
</ul>
```

```js
const b = document.getElementById('b');

b.nextSibling;           // → Text "\n  "   ← nodo de texto, no el <li>
b.nextElementSibling;    // → <li id="c">   ← el elemento correcto
b.previousSibling;       // → Text "\n  "
b.previousElementSibling;// → <li id="a">
```

HTML minificado (`<li>A</li><li>B</li>`) no genera nodos de texto entre elementos, pero no es el caso habitual en código mantenido.

## Receta: activar el tab siguiente en un componente

```js
const tabActivo = document.querySelector('.tab.activo');
const siguiente = tabActivo.nextElementSibling;

if (siguiente) {
  tabActivo.classList.remove('activo');
  siguiente.classList.add('activo');
}
```

La comprobación de `null` evita el error cuando el tab activo ya es el último.

## Receta: recopilar todos los hermanos siguientes hasta encontrar un separador

```js
function hermanosHasta(inicio, selectorStop) {
  const resultado = [];
  let actual = inicio.nextElementSibling;

  while (actual && !actual.matches(selectorStop)) {
    resultado.push(actual);
    actual = actual.nextElementSibling;
  }

  return resultado;
}

// Uso: todos los <li> después de "a" hasta el primer <li class="separador">
const grupo = hermanosHasta(
  document.getElementById('a'),
  '.separador'
);
```

## Receta: insertar un elemento después de otro sin `after()`

Antes de que `Element.after()` fuera universal, el patrón estándar era:

```js
function insertarDespues(nuevoNodo, referencia) {
  referencia.parentNode.insertBefore(nuevoNodo, referencia.nextSibling);
}
```

`nextSibling` (no `nextElementSibling`) es correcto aquí: `insertBefore` acepta `null` como segundo argumento (inserta al final) y el nodo de texto como segundo argumento también es válido.

## Cómo funciona por dentro

> [!info]
> La lista de hijos de un nodo se implementa internamente como una lista enlazada doble: cada nodo guarda punteros a su hermano anterior y al siguiente. `nextSibling` y `previousSibling` devuelven esos punteros directamente; la operación es O(1) sin ningún recorrido. `nextElementSibling` y `previousElementSibling` siguen el mismo puntero pero añaden un bucle que avanza por la lista hasta encontrar un nodo con `nodeType === 1` (Element) o llegar a `null`.
>
> En estructuras con muchos nodos de texto consecutivos (HTML muy verboso), `nextElementSibling` puede recorrer varios nodos antes de encontrar el siguiente Element, pero en HTML normal el costo es despreciable.

> [!tip] Buenas prácticas
> - Usar siempre `nextElementSibling` / `previousElementSibling` en código de producto: el salto de nodos de texto es automático y el código resulta más predecible.
> - Comprobar `null` antes de acceder a propiedades del resultado: el primer y último hermano no tienen siguiente/anterior.
> - Para navegar varios pasos de hermano en hermano con un criterio, preferir una función auxiliar (como la receta anterior) sobre encadenamientos ciegos.

> [!warning] Errores comunes
> - Usar `nextSibling` esperando el siguiente elemento visual: en HTML formateado casi siempre devuelve un nodo de texto. Usar `nextElementSibling`.
> - Asumir que `nextElementSibling` devuelve un elemento con una clase específica: devuelve el siguiente hermano cualquiera que sea. Verificar con `.matches()` o `.classList.contains()`.
> - Modificar el DOM (reordenar hermanos) mientras se itera con `nextElementSibling`: los punteros apuntan al estado actual del árbol, por lo que los cambios afectan la navegación en curso.

## Notas relacionadas

- [[index | Recorrer el DOM]]
- [[02 childNodes y children]]
- [[01 parentNode y parentElement]]
