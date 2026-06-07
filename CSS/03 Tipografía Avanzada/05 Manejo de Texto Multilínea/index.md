---
title: Manejo de Texto Multilínea — Cortes, guionado y recorte
aliases:
  - text wrapping
  - multiline text
tags:
  - css
  - api/concepto
  - tipografia
draft: false
---

# Manejo de Texto Multilínea

> [!definicion]
> Cuando el texto no cabe en su contenedor, CSS decide **cómo se comporta**: dónde rompe la línea, si parte palabras largas, si las divide con guiones, o si recorta lo que sobra con puntos suspensivos. Estas propiedades resuelven los problemas de **desbordamiento de texto**, frecuentes con contenido dinámico (URLs, nombres, traducciones).

```css
.celda {
  overflow-wrap: break-word;   /* parte palabras largas que no caben */
}
.titulo {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;     /* recorta con … */
}
```

## Mapa de la subsección

- [[01 overflow-wrap y word-wrap]] — partir palabras largas solo cuando no caben.
- [[02 word-break]] — reglas más agresivas de corte de palabra.
- [[03 hyphens]] — guionado automático según el idioma.
- [[04 line-clamp]] — limitar el texto a N líneas con `…`.
- [[05 text-overflow]] — recortar con puntos suspensivos.

## El problema central: contenido impredecible

> [!info] Las URLs y nombres rompen los layouts
> El texto controlado por el diseñador cabe; el texto **dinámico** (un email largo, una URL sin espacios, un nombre en alemán, contenido traducido) puede **desbordar** su caja y romper el layout (scroll horizontal, contenido cortado). Estas propiedades son la defensa: dicen al navegador cómo manejar texto que no esperaba. En cualquier interfaz con datos de usuario, conviene aplicarlas preventivamente.

## Las dos estrategias

| Estrategia | Propiedades | Cuándo |
|------------|-------------|--------|
| **Ajustar** (que el texto fluya) | `overflow-wrap`, `word-break`, `hyphens` | Contenido que debe verse entero |
| **Recortar** (cortar lo que sobra) | `text-overflow`, `line-clamp` | Espacio fijo, resúmenes, tarjetas |

## Notas relacionadas

- [[13 white-space]] — controla si el texto ajusta (`nowrap`, `pre-wrap`).
- [[01 overflow-wrap y word-wrap]] — la defensa más común contra desbordes.
- [[05 text-overflow]] — el recorte con `…`.
