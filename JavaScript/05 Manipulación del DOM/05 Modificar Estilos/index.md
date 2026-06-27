---
title: "Modificar Estilos — Índice"
aliases:
  - Índice Modificar Estilos
  - DOM estilos JS
tags:
  - javascript
  - dom
  - index
draft: false
order: 5
---

# Modificar Estilos

Desde JavaScript hay dos superficies principales para leer y escribir estilos de un elemento. Son complementarias: una es para escritura (estilos inline), la otra es para lectura del estado real del elemento en pantalla.

| Mecanismo | Lectura | Escritura | Fuente de estilos | Cuándo usarlo |
|---|---|---|---|---|
| `element.style` | Solo estilos inline | Sí | Atributo `style` del HTML | Aplicar/quitar estilos dinámicos, variables CSS custom properties |
| `getComputedStyle(el)` | Todos los estilos aplicados | No (solo lectura) | Hojas de estilo + herencia + cascada | Leer el valor real calculado de cualquier propiedad |

La regla práctica: usa `element.style` para **escribir** y `getComputedStyle` para **leer** el estado real. Leer `element.style.color` cuando el color viene de una clase CSS devolverá una cadena vacía — no el color visible.

## Notas relacionadas

- [[01 Propiedad style]]
- [[02 getComputedStyle]]
