---
title: direction — Dirección del texto (LTR/RTL)
aliases:
  - direction
  - rtl
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: direction
grupo: tipografia
valor_inicial: ltr
hereda: true
animable: false
draft: false
order: 2
---

# direction

> [!definicion]
> `direction` define la **dirección base** del texto en línea: de izquierda a derecha (`ltr`, español, inglés) o de derecha a izquierda (`rtl`, árabe, hebreo). Afecta al orden del texto, la alineación por defecto y el sentido de las propiedades lógicas.

```css
.arabe { direction: rtl; }
```

## Valores

| Valor | Dirección |
|-------|-----------|
| `ltr` | Izquierda → derecha (por defecto) |
| `rtl` | Derecha → izquierda |

## Mejor el atributo dir del HTML

> [!warning] Prefiere dir="rtl" en el HTML, no direction en CSS
> Aunque `direction` existe en CSS, la **forma recomendada** de fijar la direccionalidad es el atributo HTML [[04 Idioma y Dirección (lang, dir) | `dir`]], no la propiedad CSS:
> ```html
> <html dir="rtl" lang="ar">
> ```
> Razones: la direccionalidad es **estructural** (afecta a la semántica del documento, no solo al estilo), y el atributo `dir` interactúa correctamente con el algoritmo bidireccional y con formularios. Usar `direction` en CSS para esto puede dar resultados incompletos. La propiedad CSS se reserva para casos puntuales.

## Qué cambia con rtl

> [!info] rtl reorienta todo el flujo lógico
> Poner `rtl` no solo alinea el texto a la derecha: **invierte** el sentido del eje inline, lo que afecta a:
> - El **orden** del texto y de los elementos en línea (empiezan por la derecha).
> - La alineación por defecto (`text-align: start` = derecha).
> - El sentido de las propiedades **lógicas**: `margin-inline-start` pasa a ser el lado **derecho**.
> - El inicio de listas, el orden de columnas flex/grid (con propiedades lógicas).

## direction no es text-align

> [!warning] Para alinear a la derecha, usa text-align
> Un error común: usar `direction: rtl` solo para alinear texto a la derecha. `direction` cambia la **direccionalidad lógica** completa (el orden de todo), no solo la alineación. Para alinear texto a la derecha en un documento LTR, usa [[08 text-align | `text-align: right`]] (o `end`), no `direction`.

## Las propiedades lógicas lo respetan

El gran beneficio: si usas propiedades **lógicas** (`margin-inline`, `padding-inline-start`, `inset-inline`…), un cambio de `direction`/`dir` reorienta el layout **automáticamente**, sin reescribir CSS. Un `margin-inline-start: 1rem` queda a la izquierda en LTR y a la derecha en RTL, sin tocar nada.

## Buenas prácticas

> [!tip] Recomendaciones
> - Fija la direccionalidad con `dir="rtl"` en el HTML, no con `direction` en CSS.
> - Usa **propiedades lógicas** (`margin-inline`, etc.) para soportar RTL sin reescribir.
> - Para alinear texto, usa `text-align: start/end`, no `direction`.
> - Prueba la interfaz en RTL si tu sitio puede ser multilingüe.

## Errores comunes

> [!warning] Trampas
> - **`direction: rtl` para alinear** a la derecha: cambia mucho más que la alineación.
> - **Propiedades físicas** (`margin-left`) en sitios RTL: no se reorientan.
> - **`direction` en CSS** en vez del atributo `dir` para direccionalidad estructural.

## Notas relacionadas

- [[04 Idioma y Dirección (lang, dir)]] — el atributo `dir`, la forma recomendada.
- [[01 writing-mode]] — la dirección vertical, su pariente.
- [[04 unicode-bidi]] — el control fino del algoritmo bidireccional.
