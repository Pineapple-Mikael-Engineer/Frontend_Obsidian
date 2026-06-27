---
title: querySelectorAll — Todos los elementos que coinciden con un selector CSS
aliases:
  - querySelectorAll
  - Element.querySelectorAll
tags:
  - javascript
  - api/metodo
  - dom
objeto: Element
tipo: metodo
retorna: NodeList
muta: false
asincrono: false
draft: false
order: 4
---

# `querySelectorAll`

> [!definicion]
> `element.querySelectorAll(selector)` retorna una `NodeList` **estática** con todos los elementos descendientes del receptor que coinciden con el selector CSS `selector`, ordenados según su posición en el documento (depth-first, preorder). Retorna una `NodeList` vacía si no hay coincidencias (nunca `null`). Disponible en `document` y en cualquier `Element`.

```js
const tarjetas  = document.querySelectorAll('.card');
const inputs    = document.querySelectorAll('input[required]');
const titulos   = document.querySelectorAll('h1, h2, h3');
// → NodeList estática en los tres casos

console.log(tarjetas.length);          // número de coincidencias
console.log(tarjetas[0]);              // primer elemento
```

## Firma

```text
element.querySelectorAll(selector: string): NodeList
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `selector` | `string` | Selector CSS válido; acepta listas separadas por coma |

**Retorna:** `NodeList` estática. Si el selector es sintácticamente inválido, lanza `DOMException: SyntaxError`.

## NodeList estática: fotografía del DOM

La colección devuelta es una instantánea del momento en que se ejecutó la llamada. Mutaciones posteriores al DOM no la afectan:

```js
const items = document.querySelectorAll('.item');
console.log(items.length);   // → 4

document.querySelector('.item').remove();
console.log(items.length);   // → 4  (no cambia — es estática)
```

Esto la diferencia de las colecciones devueltas por `getElementsByClassName` y `getElementsByTagName`, que son live.

## Iteración

`NodeList` es iterable directamente con `for...of` y tiene `forEach` nativo. Para usar `map`, `filter` o `reduce` se convierte a array:

```js
const links = document.querySelectorAll('a[href]');

// for...of — sin conversión
for (const link of links) {
  console.log(link.href);
}

// forEach nativo
links.forEach(link => link.classList.add('visitado'));

// map, filter — requieren conversión
const hrefs = [...links].map(a => a.href);
const externos = Array.from(links).filter(a => a.href.startsWith('http'));
```

## Selector de múltiples tipos (lista separada por coma)

Un selector con coma selecciona todos los elementos que coinciden con cualquiera de los selectores listados:

```js
const titulos = document.querySelectorAll('h1, h2, h3');
// → todos los <h1>, <h2> y <h3> del documento, en orden de aparición
```

## Receta: todos los links externos

```js
const linksExternos = document.querySelectorAll('a[href^="http"]');

linksExternos.forEach(a => {
  a.setAttribute('target', '_blank');
  a.setAttribute('rel', 'noopener noreferrer');
});
```

## Receta: inputs requeridos y vacíos

```js
function obtenerCamposVaciosRequeridos(form) {
  // Selecciona inputs required cuyo value es cadena vacía
  return form.querySelectorAll('input[required][value=""], input[required]:not([value])');
}

// Uso
const form = document.querySelector('#registro');
const vacios = obtenerCamposVaciosRequeridos(form);

vacios.forEach(input => input.classList.add('error'));
console.log(`${vacios.length} campos obligatorios vacíos`);
```

> [!warning]
> El selector `[value=""]` compara el atributo HTML inicial, no la propiedad `.value` del DOM (que refleja lo que el usuario ha escrito). Para validar el contenido actual del campo, iterar con JS y comparar `input.value.trim() === ''`.

## Receta: procesar todos los elementos de datos en una tabla

```js
const filas = document.querySelectorAll('#tabla-ventas tbody tr');

const datos = [...filas].map(fila => {
  const celdas = fila.querySelectorAll('td');
  return {
    producto: celdas[0]?.textContent.trim(),
    cantidad: Number(celdas[1]?.textContent),
    total:    Number(celdas[2]?.textContent.replace(',', '.'))
  };
});
```

## Cómo funciona por dentro

El motor de selectores CSS evalúa el selector contra todos los descendientes del receptor y construye una nueva lista con los nodos coincidentes en orden de documento. Al ser estática, el motor copia las referencias a los nodos en el momento de la llamada y no registra ningún observer sobre el árbol DOM; las mutaciones posteriores son irrelevantes para la `NodeList` ya devuelta. Esto simplifica la implementación respecto a las colecciones live y elimina el coste de mantenimiento de observers, pero implica que la lista puede quedar "desactualizada" si el DOM cambia.

> [!tip] Buenas prácticas
> - Preferir `querySelectorAll` sobre `getElementsByClassName`/`getElementsByTagName` cuando la colección se va a iterar o manipular de inmediato y no se necesita que se actualice automáticamente.
> - Para listas de selectores complejos, separar con coma en lugar de hacer múltiples llamadas y concatenar arrays.
> - Acotar al subárbol mínimo necesario: `form.querySelectorAll('...')` es más eficiente que `document.querySelectorAll('...')` cuando el scope es conocido.

> [!warning] Errores comunes
> - Asumir que retorna `null` cuando no hay coincidencias: retorna una `NodeList` vacía. Comprobar `nl.length === 0`, no `nl === null`.
> - Llamar `.map()` directamente sobre la `NodeList`: `NodeList` no es un array y no tiene `map`. Convertir primero con `[...nl]` o `Array.from(nl)`.
> - Selector inválido sintácticamente: lanza `DOMException` en tiempo de ejecución, no en tiempo de compilación. Las IDEs con soporte TypeScript/DOM no validan la cadena del selector.
> - Confundir la naturaleza estática con inmutabilidad del nodo: la `NodeList` no cambia, pero los nodos que contiene sí son mutables.

## Notas relacionadas

- [[index | Selección de Elementos]]
- [[03 querySelector]]
- [[05 HTMLCollection vs NodeList]]
- [[02 getElementsByClassName y TagName]]
