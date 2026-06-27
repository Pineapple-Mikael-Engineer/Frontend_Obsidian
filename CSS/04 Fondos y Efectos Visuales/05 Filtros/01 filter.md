---
title: filter — Efectos gráficos sobre el elemento
aliases:
  - filter
  - blur
  - grayscale
tags:
  - css
  - api/propiedad
  - fondos
propiedad: filter
grupo: fondo
valor_inicial: none
hereda: false
animable: true
draft: false
order: 1
---

# filter

> [!definicion]
> `filter` aplica uno o varios **efectos gráficos** al elemento y su contenido: desenfoque, brillo, contraste, saturación, escala de grises, inversión, sombra. Los efectos se encadenan separados por espacios y se aplican en orden.

```css
.imagen { filter: grayscale(1); }
.imagen:hover { filter: grayscale(0) brightness(1.1); }
```

## Las funciones de filtro

| Función | Efecto | Rango |
|---------|--------|-------|
| `blur(px)` | Desenfoque gaussiano | `0` a ∞ |
| `brightness(n)` | Brillo | `0` (negro) `1` (normal) `>1` (más brillo) |
| `contrast(n)` | Contraste | `0` (gris) `1` (normal) `>1` (más) |
| `saturate(n)` | Saturación | `0` (gris) `1` (normal) `>1` (más vivo) |
| `grayscale(n)` | Escala de grises | `0` (color) `1` (gris total) |
| `sepia(n)` | Tono sepia | `0` a `1` |
| `invert(n)` | Invertir colores | `0` a `1` |
| `hue-rotate(deg)` | Rotar el matiz | `0deg` a `360deg` |
| `opacity(n)` | Opacidad | `0` a `1` |
| `drop-shadow(...)` | Sombra que sigue la **forma** | como box-shadow |

## Encadenar filtros

Se aplican **en orden**, lo que importa (gris y luego brillo ≠ brillo y luego gris):

```css
filter: grayscale(0.5) contrast(1.2) brightness(0.95);
```

## drop-shadow vs. box-shadow

> [!tip] drop-shadow sigue la forma real
> `filter: drop-shadow()` es como [[03 Sombras (box-shadow) | `box-shadow`]], pero con una diferencia clave: sigue la **forma real** del contenido (incluida la transparencia), no la caja rectangular. Por eso es la forma de dar sombra a un PNG transparente, un icono SVG o un elemento con `clip-path`:
> ```css
> .icono-png { filter: drop-shadow(0 2px 4px rgb(0 0 0 / 0.3)); }
> /* la sombra sigue el contorno del icono, no su caja */
> ```
> Para una caja rectangular normal, `box-shadow` es más eficiente.

## Recetas comunes

```css
/* Imagen en gris que recupera color al pasar el ratón */
.galeria img { filter: grayscale(1); transition: filter 0.3s; }
.galeria img:hover { filter: grayscale(0); }

/* Desenfocar el fondo al abrir un modal */
.modal-abierto .contenido { filter: blur(4px); }

/* Atenuar elementos deshabilitados */
:disabled { filter: grayscale(0.6) opacity(0.6); }

/* Sombra para un icono SVG/PNG transparente */
.logo { filter: drop-shadow(0 1px 2px rgb(0 0 0 / 0.25)); }
```

## Rendimiento del blur

> [!warning] blur es el filtro más caro
> `blur()` tiene un coste de composición alto, que aumenta con el tamaño del elemento. **Animar** un `blur` (de 0 a 8px) puede ir a tirones. Si necesitas animar un desenfoque, prueba el rendimiento y considera limitarlo o usar `will-change` con cuidado. Los demás filtros (brightness, grayscale) son más baratos.

## filter crea un contexto de apilamiento

> [!info] Efectos secundarios de filter
> Aplicar cualquier `filter` (distinto de `none`) crea un **contexto de apilamiento** y un **contexto de contención**, lo que puede afectar a `position: fixed` de los hijos (dejan de ser fijos respecto a la ventana) y al `z-index`. Es un efecto secundario que sorprende: un `filter` en un ancestro puede "romper" un hijo `fixed`.

## Buenas prácticas

> [!tip] Recomendaciones
> - Encadena filtros recordando que el **orden** importa.
> - `drop-shadow` para sombras que siguen la forma (PNG/SVG transparentes); `box-shadow` para cajas.
> - Anima filtros baratos (brightness, grayscale) con libertad; el `blur` con cuidado.
> - Ten presente que `filter` crea contexto de apilamiento (cuidado con hijos `fixed`).

## Errores comunes

> [!warning] Trampas
> - **Animar `blur`** sin medir: tirones.
> - **`filter` en un ancestro** que rompe un hijo `position: fixed`.
> - **Usar `box-shadow`** en un PNG transparente (sombrea la caja, no la forma): usa `drop-shadow`.

## Notas relacionadas

- [[02 backdrop-filter]] — el filtro sobre el fondo (cristal esmerilado).
- [[03 Sombras (box-shadow)]] — sombras de caja, comparadas con `drop-shadow`.
- [[07 z-index y Contexto de Apilamiento]] — el contexto que `filter` crea.
