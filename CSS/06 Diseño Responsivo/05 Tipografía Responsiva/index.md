---
title: Tipografía Responsiva — Texto que escala con la pantalla
aliases:
  - responsive typography
tags:
  - css
  - api/concepto
  - responsive
draft: false
order: 4
---

# Tipografía Responsiva

> [!definicion]
> La **tipografía responsiva** ajusta el tamaño del texto a la pantalla: ni demasiado pequeño en móvil ni desproporcionado en monitores grandes. La forma moderna es la tipografía **fluida** con [[02 clamp() para Tipografía Fluida | `clamp()`]], que escala suavemente entre un mínimo y un máximo sin media queries.

```css
h1 { font-size: clamp(1.75rem, 1rem + 4vw, 3.5rem); }
```

## Mapa de la subsección

- [[01 Unidades vw para Fuente]] — escalar la fuente con el viewport (y sus problemas).
- [[02 clamp() para Tipografía Fluida]] — el método recomendado, acotado.

## Las tres formas de hacerlo

| Método | Cómo | Valoración |
|--------|------|------------|
| Media queries | Cambiar `font-size` por breakpoint | Funciona, pero a saltos y verboso |
| `vw` puro | `font-size: 4vw` | Fluido pero **sin límites** (extremos) |
| `clamp()` | `clamp(min, fluido, max)` | **Recomendado**: fluido y acotado |

El detalle en [[01 Unidades vw para Fuente]] y [[02 clamp() para Tipografía Fluida]].

## El principio: fluido pero acotado

> [!tip] Ni saltos ni extremos
> La tipografía ideal **fluye** (sin los saltos bruscos de las media queries) pero está **acotada** (no se vuelve diminuta en móvil ni gigante en un monitor 4K). `clamp(MIN, PREFERIDO, MAX)` logra exactamente eso: escala con la pantalla en el rango intermedio y se detiene en los límites. Reemplaza varias media queries de `font-size` por una línea.

## La accesibilidad: respetar el zoom

> [!warning] Incluye rem en la parte fluida
> Una tipografía fluida que use **solo `vw`** puede ignorar el **zoom de texto** del usuario (un requisito de accesibilidad). Por eso el término preferido de `clamp()` combina una base en `rem` con un poco de `vw`:
> ```css
> font-size: clamp(1rem, 1rem + 2vw, 2rem);   /* el rem respeta el zoom */
> ```
> Detalle en [[02 clamp() para Tipografía Fluida]].

## Notas relacionadas

- [[02 clamp() para Tipografía Fluida]] — el método recomendado.
- [[07 min(), max(), clamp()]] — la función `clamp()` en general.
- [[03 font-size]] — la propiedad que se hace fluida.
