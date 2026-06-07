---
title: linear-gradient — Gradiente lineal
aliases:
  - linear-gradient
tags:
  - css
  - api/funcion
  - fondos
draft: false
---

# linear-gradient

> [!definicion]
> `linear-gradient()` genera una transición de color **a lo largo de una línea**, en una dirección o ángulo dados. Es el gradiente más usado: fondos de botones, banners, overlays y patrones se construyen con él.

```css
background: linear-gradient(135deg, #cba6f7, #89dceb);
```

## Sintaxis

```css
linear-gradient(<dirección>, <color1> <pos1>, <color2> <pos2>, ...)
```

| Parte | Ejemplo | Significa |
|-------|---------|-----------|
| Dirección | `to right`, `45deg` | Hacia dónde va el gradiente |
| Color stops | `#cba6f7 0%, #89dceb 100%` | Colores y dónde alcanzan su pureza |

## La dirección

| Valor | Dirección |
|-------|-----------|
| `to right` | De izquierda a derecha |
| `to bottom` | De arriba abajo (por defecto si se omite) |
| `to bottom right` | Hacia la esquina inferior derecha |
| `0deg` | Hacia arriba |
| `90deg` | Hacia la derecha |
| `135deg` | Hacia la esquina inferior derecha |

> [!info] Los grados van en sentido horario desde arriba
> En `linear-gradient`, `0deg` apunta **hacia arriba** y los grados crecen en **sentido horario**: `90deg` a la derecha, `180deg` abajo. Esto difiere de las matemáticas (donde 0° suele ser a la derecha y antihorario), una fuente de confusión.

## Color stops y transiciones duras

```css
/* Degradado suave de tres colores */
linear-gradient(to right, red, yellow, green);

/* Posiciones explícitas */
linear-gradient(to right, #cba6f7 20%, #89dceb 80%);

/* Transición DURA (franjas, sin mezcla): dos stops en la misma posición */
linear-gradient(to right, #cba6f7 50%, #89dceb 50%);
```

> [!tip] Franjas y rayas con stops en la misma posición
> Poniendo dos colores en la **misma** posición se crea un corte neto en vez de un degradado. Repitiéndolo, se hacen rayas:
> ```css
> /* Rayas diagonales */
> background: repeating-linear-gradient(45deg, #cba6f7 0 10px, #b48ef0 10px 20px);
> ```

## Recetas comunes

```css
/* Botón con degradado de marca */
.btn { background: linear-gradient(135deg, #cba6f7, #b48ef0); }

/* Overlay que oscurece la parte inferior de una foto (para texto) */
.card { background:
  linear-gradient(to top, rgb(0 0 0 / 0.7), transparent 50%),
  url("foto.jpg") center / cover; }

/* Línea divisoria con degradado (se desvanece en los extremos) */
hr { height: 1px; border: none;
     background: linear-gradient(to right, transparent, #cba6f7, transparent); }

/* Texto con relleno de gradiente */
.titulo {
  background: linear-gradient(135deg, #cba6f7, #f38ba8);
  background-clip: text; -webkit-background-clip: text; color: transparent;
}
```

## Interpolación en oklch

> [!tip] in oklch para degradados vibrantes
> Para evitar el "gris turbio" entre colores opuestos, interpola en OKLCH:
> ```css
> background: linear-gradient(in oklch, #cba6f7, #89dceb);
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Recuerda que `0deg` es **arriba** y los grados van en horario.
> - Usa stops en la misma posición para franjas/rayas netas.
> - Interpola `in oklch` para transiciones vivas.
> - Combínalo con [[09 Múltiples Fondos | varias capas]] para overlays y patrones.
> - Define los colores en variables para tematizar.

## Errores comunes

> [!warning] Trampas
> - **Confundir el ángulo** (0° es arriba, no a la derecha).
> - **Gris turbio** al degradar en `srgb` entre colores opuestos: usa `in oklch`.
> - **Olvidar `color: transparent`** al usar `background-clip: text`.

## Notas relacionadas

- [[02 radial-gradient]] · [[03 conic-gradient]] — los otros tipos.
- [[04 Gradientes Repetidos]] — `repeating-linear-gradient` para rayas.
- [[07 background-clip y background-origin]] — el truco de texto con gradiente.
