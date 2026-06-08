---
title: "createElement y createTextNode"
aliases:
  - document.createElement
  - document.createTextNode
  - crear elementos DOM
tags:
  - javascript
  - api/metodo
  - dom
draft: false
---

# `createElement` y `createTextNode`

> [!definicion]
> `document.createElement(tagName)` crea un nuevo elemento HTML del tipo indicado. El elemento existe en memoria como un objeto JavaScript completo, con todos los métodos y propiedades de su tipo, pero no está en el árbol del documento — no se renderiza ni afecta el layout hasta que se inserta con un método de inserción. `document.createTextNode(texto)` crea un nodo de texto puro, con escape automático del contenido.

```js
// Crear un elemento
const div = document.createElement("div");
const li = document.createElement("li");
const input = document.createElement("input");
const svg = document.createElementNS("http://www.w3.org/2000/svg", "svg"); // para SVG

// Crear un nodo de texto
const texto = document.createTextNode("Hola <mundo>"); // el < y > se escapan automáticamente
```

## Configurar antes de insertar

El momento óptimo para configurar el elemento es **antes de insertarlo** en el DOM. Hacerlo después provoca reflows adicionales por cada modificación.

```js
const li = document.createElement("li");

// Configurar todo antes de insertar
li.textContent = "Nuevo item";
li.id = "item-7";
li.className = "item item--activo";
li.classList.add("item--destacado");
li.setAttribute("data-id", "7");
li.style.color = "steelblue";

// Una sola inserción — un solo reflow
lista.appendChild(li);
```

## `document.createTextNode`

Crea un nodo de texto cuyo contenido es el string proporcionado, con escape automático de caracteres HTML. Esto lo hace seguro ante XSS cuando el texto proviene de datos del usuario.

```js
const usuario = "<script>alert('xss')</script>"; // input malicioso
const nodoTexto = document.createTextNode(usuario);
// El resultado en el DOM: texto literal, no código ejecutable

el.appendChild(nodoTexto);
// Equivale a: el.textContent = usuario  (ambos escapan automáticamente)
```

Usar `createTextNode` es equivalente a asignar `textContent`. La alternativa insegura sería `innerHTML`, que interpreta el HTML sin escape.

## Métodos de creación relacionados

| Método | Crea | Nota |
|---|---|---|
| `document.createElement(tag)` | Elemento HTML | El más común |
| `document.createTextNode(str)` | Nodo de texto | Seguro ante XSS, escapa automáticamente |
| `document.createDocumentFragment()` | Fragmento virtual | Contenedor temporal sin padre real |
| `nodo.cloneNode(deep)` | Copia de un nodo existente | Ver nota dedicada |
| `document.createElementNS(ns, tag)` | Elemento con namespace | Necesario para SVG y MathML |

## Receta: crear un `<li>` completo y añadirlo a una lista

```js
function crearItem(texto, id) {
  const li = document.createElement("li");
  li.textContent = texto;
  li.dataset.id = id;
  li.classList.add("lista__item");
  return li; // devolver para que el llamador decida dónde insertar
}

const lista = document.querySelector(".lista");
lista.appendChild(crearItem("Primer elemento", 1));
lista.appendChild(crearItem("Segundo elemento", 2));
```

## Receta: crear estructura anidada

```js
function crearTarjeta({ titulo, descripcion, imagen }) {
  const article = document.createElement("article");
  article.classList.add("tarjeta");

  const img = document.createElement("img");
  img.src = imagen;
  img.alt = titulo;

  const h2 = document.createElement("h2");
  h2.textContent = titulo;

  const p = document.createElement("p");
  p.textContent = descripcion;

  article.append(img, h2, p); // append acepta múltiples nodos
  return article;
}
```

## Cómo funciona por dentro

Cuando se llama a `document.createElement("li")`, el motor:

1. Consulta el registro de elementos HTML para determinar el constructor correcto (`HTMLLIElement`, `HTMLInputElement`, etc.).
2. Crea una instancia del objeto con el prototipo adecuado — el elemento hereda todos los métodos del tipo específico y de `HTMLElement`, `Element`, `Node`.
3. Asocia el elemento al documento (`el.ownerDocument === document`) pero sin padre ni posición en el árbol (`el.parentNode === null`, `el.isConnected === false`).
4. No se ejecuta ningún recálculo de estilos ni reflow — el elemento no existe para el motor de rendering hasta que se inserta.

Una vez insertado, el motor marca el subárbol como "sucio" para el próximo frame de rendering y aplica los estilos correspondientes.

> [!tip]
> Devuelve el elemento recién creado desde una función de fábrica en lugar de insertarlo directamente dentro de la función. Esto desacopla la creación de la inserción y hace el código más testeable y reutilizable — el llamador decide el destino.

> [!warning]
> Nunca uses `innerHTML` para crear elementos a partir de datos del usuario. `createElement` + `textContent` o `createTextNode` son las alternativas seguras. Si necesitas crear HTML estructurado desde un string de confianza, considera `template.innerHTML` + `importNode` o la API de `DOMParser`.

## Notas relacionadas

- [[02 appendChild y append]]
- [[06 cloneNode]]
- [[index|Crear e Insertar Elementos — Índice]]
- [[../03 Modificar Contenido/index|Modificar Contenido]]
