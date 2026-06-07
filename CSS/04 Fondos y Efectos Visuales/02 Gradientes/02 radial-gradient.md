---
title: radial-gradient — Gradiente radial
aliases:
  - radial-gradient
tags:
  - css
  - api/funcion
  - fondos
draft: false
---

# radial-gradient

> [!definicion]
> `radial-gradient()` genera una transición de color que **irradia desde un punto** hacia fuera, en forma de círculo o elipse. Sirve para focos de luz, viñetas, halos, fondos suaves centrados y patrones de puntos.

```css
background: radial-gradient(circle, #cba6f7, #1e1e2e);
```

## Sintaxis

```css
radial-gradient(<forma> <tamaño> at <posición>, <color1>, <color2>, ...)
```

| Parte | Ejemplo | Significa |
|-------|---------|-----------|
| Forma | `circle` / `ellipse` | Círculo o elipse (elipse por defecto) |
| Tamaño | `closest-side`, `farthest-corner` | Hasta dónde llega el gradiente |
| Posición | `at center`, `at 30% 40%` | El centro del que irradia |

```css
radial-gradient(circle at top left, #cba6f7, transparent);
```

## La forma y el tamaño

| Tamaño | El gradiente termina en… |
|--------|--------------------------|
| `closest-side` | El lado más cercano al centro |
| `farthest-side` | El lado más lejano |
| `closest-corner` | La esquina más cercana |
| `farthest-corner` | La esquina más lejana (por defecto) |

> [!info] circle vs. ellipse
> Por defecto, un `radial-gradient` es una **elipse** que se ajusta a la proporción de la caja (más ancho que alto en cajas anchas). `circle` fuerza un **círculo** perfecto. Para un foco circular uniforme, usa `circle`:
> ```css
> radial-gradient(circle, ...)   /* círculo */
> radial-gradient(ellipse, ...)  /* elipse (por defecto) */
> ```

## Recetas comunes

```css
/* Foco de luz / halo suave */
.foco { background: radial-gradient(circle at center, rgb(255 255 255 / 0.4), transparent 60%); }

/* Viñeta (oscurece los bordes) */
.vineta { background: radial-gradient(ellipse at center, transparent 50%, rgb(0 0 0 / 0.6)); }

/* Patrón de puntos (con background-size) */
.puntos {
  background-image: radial-gradient(#cba6f7 2px, transparent 2px);
  background-size: 20px 20px;
}

/* Botón con brillo desde una esquina */
.btn-glow { background: radial-gradient(circle at top left, #d8bbfc, #cba6f7); }
```

## El patrón de puntos

> [!tip] Puntos con radial-gradient + background-size
> Una textura muy usada: un punto (`radial-gradient` pequeño) repetido en una cuadrícula que define `background-size`:
> ```css
> background-image: radial-gradient(#cba6f7 1.5px, transparent 1.5px);
> background-size: 16px 16px;   /* un punto cada 16px */
> ```
> El primer stop (el punto) y el segundo (transparente) controlan el tamaño del punto; `background-size`, la separación.

## Buenas prácticas

> [!tip] Recomendaciones
> - `circle` para focos uniformes; `ellipse` (por defecto) se adapta a la caja.
> - `at <posición>` para mover el centro (esquinas, focos descentrados).
> - Combínalo con `background-size` para patrones de puntos.
> - Usa stops con `transparent` para halos y viñetas que se desvanecen.

## Errores comunes

> [!warning] Trampas
> - **Esperar un círculo** sin poner `circle` (por defecto es elipse adaptada a la caja).
> - **Patrón de puntos sin `background-size`**: un solo punto gigante centrado.
> - **Gris turbio**: interpola `in oklch` si degradas entre colores opuestos.

## Notas relacionadas

- [[01 linear-gradient]] — el gradiente lineal.
- [[03 conic-gradient]] — el gradiente cónico (giratorio).
- [[05 background-size]] — la cuadrícula de los patrones de puntos.
