---
title: Modificar Contenido del DOM — textContent, innerHTML, innerText, outerHTML, insertAdjacentHTML
aliases:
  - Modificar Contenido
  - DOM content modification
tags:
  - javascript
  - api/concepto
  - dom
objeto: Element
tipo: concepto
muta: true
asincrono: false
draft: false
---

# Modificar Contenido del DOM

> [!definicion]
> Las propiedades de contenido del DOM permiten leer y escribir el texto o HTML interno de un elemento. La elección entre ellas determina si el resultado es seguro ante XSS, si respeta el renderizado CSS, y si reemplaza o inserta sin destruir los nodos existentes.

La regla operativa:
- `textContent` — para texto plano; seguro ante XSS; incluye nodos ocultos.
- `innerHTML` — para HTML; potente pero peligroso con datos del usuario.
- `innerText` — para el texto visible tal como lo ve el usuario; más lento.
- `outerHTML` — como `innerHTML` pero incluye el propio elemento; reemplaza el nodo.
- `insertAdjacentHTML` — para insertar HTML en una posición sin destruir el contenido existente.

## Tabla comparativa

| Propiedad / Método | Lee / escribe | Parsea HTML | Seguro ante XSS | Incluye nodos ocultos | Reemplaza nodo raíz |
|--------------------|---------------|-------------|-----------------|----------------------|---------------------|
| `textContent` | Texto plano | No | Sí — escapa `<`, `>`, `&` | Sí (`display:none`, `<script>`) | No |
| `innerHTML` | HTML interno | Sí | No — inyecta HTML literal | Sí | No |
| `innerText` | Texto renderizado | No | Sí | No — omite `display:none` | No |
| `outerHTML` | HTML completo del nodo | Sí (al escribir) | No | Sí | Sí — elimina el nodo original |
| `insertAdjacentHTML` | Solo escribe | Sí | No — mismo riesgo que `innerHTML` | — | No — inserta sin borrar |

## Cuándo usar cada uno

`textContent` es la opción por defecto para mostrar datos del usuario o limpiar un elemento. `innerHTML` sirve para inyectar plantillas HTML controladas. `insertAdjacentHTML` es preferible a `innerHTML +=` porque no destruye los hijos existentes ni pierde sus event listeners. `outerHTML` se reserva para reemplazar un elemento por otro tipo diferente.

## Notas relacionadas

- [[01 textContent]]
- [[02 innerHTML (XSS)]]
- [[03 innerText]]
- [[04 outerHTML]]
- [[05 insertAdjacentHTML]]
- [[06 Crear e Insertar Elementos/index | Crear e Insertar Elementos]]
