---
title: writing-mode — Dirección horizontal o vertical del texto
aliases:
  - writing-mode
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: writing-mode
grupo: tipografia
valor_inicial: horizontal-tb
hereda: true
animable: false
draft: false
order: 1
---

# writing-mode

> [!definicion]
> `writing-mode` define si el texto fluye **horizontalmente** (líneas apiladas verticalmente, como el español) o **verticalmente** (líneas apiladas en columnas, como el japonés tradicional), y en qué sentido. Cambia los ejes fundamentales del layout: lo que era "ancho" pasa a ser "alto".

```css
.vertical { writing-mode: vertical-rl; }   /* columnas, de derecha a izquierda */
```

## Valores

| Valor | Flujo del texto | Líneas se apilan… |
|-------|-----------------|--------------------|
| `horizontal-tb` | Horizontal (por defecto) | De arriba abajo |
| `vertical-rl` | Vertical, líneas de derecha a izquierda | De derecha a izquierda |
| `vertical-lr` | Vertical, líneas de izquierda a derecha | De izquierda a derecha |
| `sideways-rl` / `sideways-lr` | Texto girado 90° (latino vertical) | — |

## Intercambia los ejes inline y block

> [!info] "Ancho" y "alto" cambian de sentido
> El efecto profundo de `writing-mode`: redefine los ejes **inline** (dirección del texto) y **block** (dirección de apilado de líneas). En `horizontal-tb`, inline = horizontal y block = vertical. En `vertical-rl`, **se intercambian**: inline = vertical, block = horizontal. Por eso las propiedades lógicas (`inline-size`, `block-size`, `margin-block`…) se reorientan solas, mientras que `width`/`height` mantienen su sentido físico y pueden descuadrar el layout vertical.

## Usos

| Uso | writing-mode |
|-----|--------------|
| Texto japonés/chino tradicional | `vertical-rl` |
| Etiquetas de eje en un gráfico | `vertical-rl` o `sideways-lr` |
| Encabezados verticales en una tabla | `vertical-rl` |
| Diseño editorial creativo | varios |

## Texto latino vertical: sideways

Para poner texto **latino** en vertical (una etiqueta girada), `vertical-rl` orienta cada letra de forma extraña; `sideways-lr` gira todo el texto 90° manteniéndolo legible:

```css
.etiqueta-eje {
  writing-mode: sideways-lr;   /* texto latino girado, legible */
}
```

(El soporte de `sideways-*` es más limitado que el de `vertical-*`.)

## Combinar con text-orientation

En modo vertical, [[03 text-orientation | `text-orientation`]] decide si cada glifo se mantiene **vertical** (apilado, para CJK) o se **gira** de lado (para texto latino). Las dos propiedades trabajan juntas para la escritura vertical.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `vertical-rl` para texto CJK tradicional.
> - Para etiquetas latinas verticales, `sideways-lr` (más legible que `vertical-rl`).
> - Usa **propiedades lógicas** (no `width`/`height`) para que el layout se reoriente solo.
> - Combina con `text-orientation` para controlar la orientación de los glifos.

## Errores comunes

> [!warning] Trampas
> - **Usar `width`/`height`** en layout vertical: no se reorientan; usa `inline-size`/`block-size`.
> - **`vertical-rl` con texto latino**: las letras quedan de lado de forma rara (usa `sideways`).
> - **Olvidar `text-orientation`** al mezclar latín y CJK en vertical.

## Notas relacionadas

- [[03 text-orientation]] — orientación de los glifos en vertical.
- [[02 direction]] — dirección del texto en línea (LTR/RTL).
- [[index]] — propiedades lógicas vs. físicas.
