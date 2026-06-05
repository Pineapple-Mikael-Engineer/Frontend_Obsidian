---
title: Texto para Iconos — Nombre accesible de iconos y botones de icono
aliases:
  - icon accessibility
  - botones de icono
tags:
  - html
  - api/concepto
  - a11y
draft: false
---

# Texto para Iconos

> [!definicion]
> Los **iconos** (un corazón, una lupa, una papelera) transmiten significado visualmente, pero son invisibles para los lectores de pantalla salvo que se les dé un **nombre textual**. Un botón de solo icono sin nombre es uno de los fallos de accesibilidad más extendidos.

```html
<!-- ✅ Botón de icono con nombre accesible -->
<button aria-label="Eliminar">
  <svg aria-hidden="true">…</svg>
</button>
```

## Las dos piezas del patrón

> [!info] Nombrar el control + ocultar el icono
> Un botón de solo icono accesible combina dos cosas:
> 1. **Nombre en el control**: `aria-label` (o texto oculto) en el `<button>`, que describe la **acción**.
> 2. **Icono oculto a la asistencia**: `aria-hidden="true"` en el `<svg>`/icono, para que no se anuncie por separado.
>
> Así el lector anuncia "Eliminar, botón" (la acción), no el dibujo. Sin el `aria-hidden`, podría intentar describir el SVG; sin el `aria-label`, anunciaría un botón **sin nombre**.

## Tres formas de nombrar

| Técnica | Cómo |
|---------|------|
| `aria-label` | `<button aria-label="Buscar">` — texto directo |
| Texto oculto | Texto visible solo para lectores (clase `sr-only`) |
| Texto visible | Mostrar también una etiqueta junto al icono (lo más claro para todos) |

```html
<!-- aria-label -->
<button aria-label="Cerrar">✕</button>

<!-- texto oculto -->
<button><svg aria-hidden="true">…</svg><span class="sr-only">Cerrar</span></button>

<!-- texto visible (ideal cuando hay espacio) -->
<button><svg aria-hidden="true">…</svg> Cerrar</button>
```

## El icono solo es decorativo

Un icono que **acompaña** a un texto visible (un disquete junto a "Guardar") es **decorativo**: el texto ya nombra la acción, así que el icono se oculta con `aria-hidden="true"` para no duplicar:

```html
<button><svg aria-hidden="true">💾</svg> Guardar</button>
<!-- el lector anuncia "Guardar", no "imagen Guardar" -->
```

## Iconos de fuente y emojis

| Caso | Tratamiento |
|------|-------------|
| Icono SVG inline | `aria-hidden="true"` + nombre en el control |
| Icono de fuente (`<i class="fa-...">`) | `aria-hidden="true"`; el carácter no debe leerse |
| Emoji con significado | `role="img"` + `aria-label` describiendo el emoji |

## Buenas prácticas

> [!tip] Recomendaciones
> - Todo botón de solo icono necesita un nombre accesible (`aria-label` o texto oculto).
> - Oculta el icono decorativo con `aria-hidden="true"`.
> - Si hay espacio, **muestra texto visible** junto al icono: es lo mejor para todos, no solo para lectores.
> - El nombre describe la **acción/destino**, no el dibujo.

## Errores comunes

> [!warning] Trampas
> - **Botón de icono sin nombre**: el lector anuncia "botón" sin más.
> - **Icono sin `aria-hidden`** junto a texto: se anuncia dos veces.
> - **Nombrar el dibujo** ("disquete") en vez de la acción ("Guardar").

## Notas relacionadas

- [[04 Propiedades ARIA (label, labelledby, describedby)]] — `aria-label` en detalle.
- [[03 Contenido Oculto Visualmente (sr-only)]] — la técnica del texto oculto.
- [[02 Gráficos Vectoriales (svg)]] — los iconos SVG y su accesibilidad.
