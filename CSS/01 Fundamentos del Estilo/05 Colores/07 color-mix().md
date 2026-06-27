---
title: color-mix() — Mezclar dos colores
aliases:
  - color-mix
tags:
  - css
  - api/funcion
  - fondos
draft: false
order: 7
---

# color-mix()

> [!definicion]
> La función `color-mix()` mezcla **dos colores** en una proporción dada, dentro de un espacio de color que tú eliges. Permite generar variantes (aclarar, oscurecer, teñir) directamente en CSS, sin preprocesadores ni cálculos manuales.

```css
color-mix(in oklch, #cba6f7 70%, white);
/* 70% del morado + 30% de blanco = una versión más clara */
```

## Anatomía

```css
color-mix(in <espacio>, <color1> <p1>%, <color2> <p2>%)
```

| Parte | Qué es |
|-------|--------|
| `in <espacio>` | El espacio donde se interpola (`oklch`, `srgb`, `hsl`, `lab`…) |
| `<color1> <p1>%` | Primer color y su proporción |
| `<color2> <p2>%` | Segundo color y su proporción |

Si omites un porcentaje, se reparte el resto; si omites ambos, es 50/50:

```css
color-mix(in oklch, red, blue)         /* 50% rojo, 50% azul */
color-mix(in oklch, red 30%, blue)     /* 30% rojo, 70% azul */
```

## El espacio importa (mucho)

> [!warning] Mezclar en sRGB puede dar grises sucios
> El espacio de interpolación cambia el resultado de forma notable. Mezclar dos colores vivos opuestos en `srgb` suele pasar por un **gris apagado** en el medio; en `oklch` u `oklab`, la transición mantiene los colores **vivos y naturales**:
> ```css
> color-mix(in srgb, blue, yellow)    /* gris verdoso turbio */
> color-mix(in oklch, blue, yellow)   /* verde vivo, mejor */
> ```
> Por eso, salvo motivo concreto, **mezcla en `oklch` o `oklab`**.

## Recetas habituales

```css
/* Aclarar un color (teñir con blanco) */
color-mix(in oklch, var(--acento) 80%, white)

/* Oscurecer (teñir con negro) */
color-mix(in oklch, var(--acento) 80%, black)

/* Estado hover: un 15% más oscuro */
.boton:hover { background: color-mix(in oklch, var(--btn) 85%, black); }

/* Versión transparente de un color */
color-mix(in srgb, var(--acento) 20%, transparent)
```

## La gran ventaja: variables dinámicas

> [!tip] Paletas a partir de una sola variable
> `color-mix()` brilla con [[10 Variables CSS/index | variables CSS]]: defines **un** color de marca y derivas todas las variantes (claro, oscuro, hover, fondo tenue) en CSS puro, sin preprocesador. Cambiar la variable base recalcula toda la familia:
> ```css
> :root { --marca: oklch(0.7 0.15 300); }
> .btn        { background: var(--marca); }
> .btn:hover  { background: color-mix(in oklch, var(--marca) 85%, black); }
> .btn-tenue  { background: color-mix(in oklch, var(--marca) 15%, white); }
> ```

## Soporte

`color-mix()` tiene buen soporte en navegadores modernos. Para navegadores antiguos, se da un respaldo con el color ya calculado antes de la declaración con `color-mix()`.

## Buenas prácticas

> [!tip] Recomendaciones
> - Mezcla en `oklch`/`oklab` para transiciones vivas; evita `srgb` para colores opuestos.
> - Combínalo con variables para generar paletas desde un color base.
> - Úsalo para estados (hover, activo) y tintes (con `white`/`black`/`transparent`).
> - Da un respaldo si soportas navegadores viejos.

## Errores comunes

> [!warning] Trampas
> - **Mezclar en `srgb`** colores opuestos: resultado grisáceo.
> - **Olvidar `in <espacio>`**: el espacio es obligatorio.
> - **Porcentajes que no cuadran**: si suman ≠ 100%, se normalizan.

## Notas relacionadas

- [[06 LAB y LCH]] — el espacio recomendado para mezclar (OKLCH/OKLab).
- [[10 Variables CSS/index]] — la base de las paletas dinámicas.
- [[06 Temas Dinámicos]] — temas construidos con mezcla de colores.
