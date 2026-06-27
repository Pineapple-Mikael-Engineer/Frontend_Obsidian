---
title: textContent — Leer y escribir texto plano de un elemento
aliases:
  - textContent
  - Node.textContent
tags:
  - javascript
  - api/metodo
  - dom
objeto: Node
tipo: metodo
retorna: string
muta: true
asincrono: false
draft: false
order: 1
---

# textContent

> [!definicion]
> `elemento.textContent` lee la concatenación de los `nodeValue` de todos los nodos de texto del subárbol, sin etiquetas HTML. Al escribir, reemplaza todos los hijos del elemento con un único nodo de texto — los caracteres `<`, `>` y `&` se escapan automáticamente, lo que lo hace seguro ante XSS.

```js
const p = document.querySelector("p");

// Leer
console.log(p.textContent);
// → "Precio: 42 €"  (sin tags, concatenación de todos los nodos de texto)

// Escribir — seguro: los < > se escapan
p.textContent = "<strong>Hola</strong>";
// El elemento muestra el texto literal: <strong>Hola</strong>
// No se crea ningún elemento <strong>
```

## Comportamiento al leer

La propiedad recorre el subárbol completo del elemento y concatena los `nodeValue` de cada nodo `Text`. Incluye texto en elementos con `display:none` y texto dentro de `<script>` y `<style>`. No filtra por visibilidad — eso lo hace [[03 innerText]].

```js
const div = document.querySelector("#contenedor");
// HTML: <div id="contenedor"><span>visible</span><span style="display:none">oculto</span></div>

console.log(div.textContent); // → "visibleoculto"
console.log(div.innerText);   // → "visible"
```

## Comportamiento al escribir

Al asignar un valor, el motor destruye todos los nodos hijos del elemento y crea un único nodo `Text` con el string dado. Es más eficiente que `innerHTML` para texto plano porque no activa el parser HTML.

```js
const lista = document.querySelector("ul");
lista.textContent = ""; // Elimina todos los <li> — forma idiomática de vaciar
```

> [!tip]
> Para limpiar el contenido de un elemento, `el.textContent = ""` es la forma más directa. Para muchos hijos con event listeners adjuntos, `el.replaceChildren()` es equivalente y más explícita.

## Tabla comparativa: textContent vs innerText

| Aspecto | `textContent` | `innerText` |
|---------|--------------|-------------|
| Incluye `display:none` | Sí | No |
| Incluye `<script>` y `<style>` | Sí | No |
| Saltos de línea por bloques | No | Sí — `\n` para `<br>` y bloques |
| Fuerza reflow | No | Sí |
| Velocidad | Rápido | Más lento |
| Disponible en nodos genéricos | Sí (`Node`) | No (solo `HTMLElement`) |

## Cómo funciona por dentro

`textContent` es una propiedad definida en la interfaz `Node` (no solo `Element`), por lo que también aplica a nodos de texto y comentarios. Internamente el motor recorre el subárbol en orden de documento y concatena los `nodeValue` de los nodos `TEXT_NODE`. Al escribir, equivale a llamar `document.createTextNode(valor)` y reemplazar todos los hijos con ese único nodo.

> [!warning]
> `textContent` en un nodo `Text` individual devuelve (y permite escribir) el propio valor del nodo, no el subárbol. En un `Element`, opera sobre todos sus descendientes.

## Notas relacionadas

- [[03 innerText]]
- [[02 innerHTML (XSS)]]
- [[03 Modificar Contenido/index | Modificar Contenido]]
