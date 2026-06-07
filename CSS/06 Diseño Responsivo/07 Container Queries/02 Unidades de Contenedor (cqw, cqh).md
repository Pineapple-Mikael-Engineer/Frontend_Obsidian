---
title: Unidades de Contenedor — cqw, cqh, cqi, cqb
aliases:
  - container query units
  - cqw
  - cqi
tags:
  - css
  - api/concepto
  - responsive
draft: false
---

# Unidades de Contenedor (cqw, cqh)

> [!definicion]
> Las **unidades de contenedor** dimensionan elementos respecto al tamaño del **contenedor de consulta**, igual que `vw`/`vh` lo hacen respecto al viewport. `1cqw` es el 1% del ancho del contenedor. Permiten que tamaños y tipografía escalen con el **contenedor**, no con la ventana.

```css
.tarjeta-wrap { container-type: inline-size; }
.tarjeta h2 { font-size: clamp(1rem, 5cqw, 2rem); }   /* escala con el contenedor */
```

## Las unidades

| Unidad | Relativa a | Análoga a (viewport) |
|--------|------------|----------------------|
| `cqw` | 1% del **ancho** del contenedor | `vw` |
| `cqh` | 1% del **alto** del contenedor | `vh` |
| `cqi` | 1% del tamaño **en línea** (lógico) | — |
| `cqb` | 1% del tamaño de **bloque** (lógico) | — |
| `cqmin` / `cqmax` | El menor/mayor de cqi y cqb | `vmin`/`vmax` |

`cqi` (inline, normalmente el ancho) es la más usada, como `cqw`.

## Tipografía que escala con el contenedor

> [!tip] El gran caso de uso: texto relativo al componente
> Las unidades de contenedor brillan en tipografía fluida **a nivel de componente**: el tamaño del título de una tarjeta escala con el **ancho de la tarjeta**, no con la pantalla. Así la misma tarjeta tiene un título proporcionado tanto a ancho completo como en una columna estrecha:
> ```css
> .card-wrap { container-type: inline-size; }
> .card h2 { font-size: clamp(1.1rem, 6cqi, 1.75rem); }
> ```
> Una tarjeta grande tiene un título grande; la misma tarjeta en una barra lateral estrecha, uno pequeño. Algo imposible con `vw` (que solo conoce el viewport).

## cqi vs. vw

> [!info] El contenedor vs. la ventana
> La diferencia esencial:
> - **`vw`**: el 1% del **viewport**. Todos los componentes escalan igual, sin importar dónde estén.
> - **`cqi`**: el 1% del **contenedor**. Cada componente escala según el espacio que ocupa.
>
> Con `vw`, dos tarjetas idénticas en columnas de distinto ancho tendrían el **mismo** tamaño de texto; con `cqi`, cada una se ajusta a su columna. `cqi` es lo que hace los componentes verdaderamente autónomos.

## Requiere un contenedor declarado

> [!warning] Las unidades cq* necesitan container-type
> Las unidades de contenedor solo funcionan dentro de un elemento que tenga un **ancestro con `container-type`** declarado. Sin él, no hay contenedor de referencia y la unidad no resuelve (cae a un comportamiento por defecto). Es el mismo prerrequisito que las reglas [[01 @container y container-type | `@container`]].

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `cqi` (ancho del contenedor) para tipografía y espaciado a nivel de componente.
> - Combina con `clamp()` para acotar (`clamp(1rem, 5cqi, 2rem)`).
> - Recuerda declarar `container-type` en el ancestro o las unidades no funcionan.
> - `cqi` para componentes autónomos; `vw` para escala global respecto a la pantalla.

## Errores comunes

> [!warning] Trampas
> - **Usar `cq*` sin un contenedor** con `container-type`: no resuelven.
> - **Confundir `cqi` con `vw`**: uno es del contenedor, otro del viewport.
> - **`cqi` puro** sin `clamp()`: sin límites en contenedores extremos.

## Notas relacionadas

- [[01 @container y container-type]] — declarar el contenedor que estas unidades miden.
- [[03 Relativas al Viewport (vw, vh, vmin, vmax)]] — las unidades de viewport, análogas.
- [[02 clamp() para Tipografía Fluida]] — acotar las unidades de contenedor.
