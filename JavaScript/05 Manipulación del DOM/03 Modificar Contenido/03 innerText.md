---
title: innerText — Texto renderizado visible de un elemento
aliases:
  - innerText
  - HTMLElement.innerText
tags:
  - javascript
  - api/metodo
  - dom
objeto: HTMLElement
tipo: metodo
retorna: string
muta: true
asincrono: false
draft: false
---

# innerText

> [!definicion]
> `elemento.innerText` lee el texto **renderizado** del elemento tal como lo ve el usuario: excluye el texto de elementos con `display:none` o `visibility:hidden`, convierte `<br>` y bloques en `\n`, y omite el contenido de `<script>` y `<style>`. Al escribir, se comporta igual que [[01 textContent]] — reemplaza todos los hijos con un nodo de texto escapado.

```js
const div = document.querySelector("#card");
// HTML: <div id="card"><p>Precio:</p><span style="display:none">oculto</span><strong> 42 €</strong></div>

console.log(div.innerText);   // → "Precio:\n 42 €"
console.log(div.textContent); // → "Precio:oculto 42 €"
```

## Por qué es más lento que textContent

`innerText` fuerza un **reflow** (recálculo de layout) porque necesita saber qué elementos están visibles y cómo se renderiza el texto — requiere conocer los estilos computados. `textContent` trabaja solo con el árbol de nodos, sin consultar el motor de layout.

En bucles o código de rendimiento crítico, usar [[01 textContent]] en su lugar.

## Conversión de estructura a texto

Al leer, `innerText` traduce la estructura visual a saltos de línea:

```js
// HTML: <div><p>Línea 1</p><p>Línea 2</p><p>Línea 3<br>misma línea</p></div>
div.innerText;
// → "Línea 1\n\nLínea 2\n\nLínea 3\nmisma línea"
// Los <p> generan \n\n (separación de bloque); <br> genera \n
```

## Tabla comparativa: innerText vs textContent

| Aspecto | `innerText` | `textContent` |
|---------|-------------|---------------|
| Elementos con `display:none` | Excluye | Incluye |
| `<script>` y `<style>` | Excluye | Incluye |
| Saltos de línea por `<br>` y bloques | Sí — `\n` | No |
| Fuerza reflow | Sí | No |
| Velocidad | Más lento | Rápido |
| Disponible en | `HTMLElement` | `Node` (más genérico) |
| Seguro ante XSS al escribir | Sí | Sí |

## Cuándo usar innerText

El caso de uso principal es obtener el texto tal como lo ve el usuario: copiar contenido al portapapeles, parsear el texto visible de una tabla, o generar un resumen de texto de un bloque HTML. Si el elemento tiene contenido oculto que no debe incluirse, `innerText` lo excluye automáticamente.

```js
// Copiar el texto visible de una tarjeta al portapapeles
document.querySelector("#copiar").addEventListener("click", () => {
  navigator.clipboard.writeText(document.querySelector(".card").innerText);
});
```

> [!warning]
> `innerText` solo existe en `HTMLElement`, no en nodos genéricos (`Node`, `SVGElement`). Acceder a `innerText` en un `SVGElement` devuelve `undefined`.

> [!info]
> `innerText` está definido en la especificación HTML (WHATWG), no en DOM Core. Existía antes en navegadores como extensión no estándar, pero su comportamiento fue estandarizado y hoy es consistente en todos los navegadores modernos.

## Notas relacionadas

- [[01 textContent]]
- [[02 innerHTML (XSS)]]
- [[03 Modificar Contenido/index | Modificar Contenido]]
