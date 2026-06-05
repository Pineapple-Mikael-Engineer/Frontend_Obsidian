---
title: <svg> — Gráficos vectoriales escalables
aliases:
  - svg element
  - scalable vector graphics
tags:
  - html
  - api/elemento
  - multimedia
elemento: svg
categoria: incrustado
rol_implicito: ninguno
vacio: false
draft: false
---

# Gráficos Vectoriales (svg)

> [!definicion]
> `<svg>` (Scalable Vector Graphics) describe gráficos **vectoriales** con etiquetas: formas geométricas definidas por coordenadas y fórmulas, no por píxeles. Escalan a cualquier tamaño sin perder nitidez, son código (estilizable y animable con CSS/JS) y cada forma vive en el DOM, lo que los hace accesibles. Ideal para iconos, logos, ilustraciones y diagramas.

```html
<svg viewBox="0 0 100 100" width="100" role="img" aria-label="Círculo morado">
  <circle cx="50" cy="50" r="40" fill="#cba6f7" stroke="#1e1e2e" stroke-width="3" />
</svg>
```

## Formas básicas

| Elemento | Dibuja |
|----------|--------|
| `<rect>` | Rectángulo (`x`, `y`, `width`, `height`) |
| `<circle>` | Círculo (`cx`, `cy`, `r`) |
| `<ellipse>` | Elipse (`cx`, `cy`, `rx`, `ry`) |
| `<line>` | Línea (`x1`, `y1`, `x2`, `y2`) |
| `<path>` | Trazado arbitrario (la más potente, con el atributo `d`) |
| `<text>` | Texto |
| `<g>` | Grupo de formas (para transformar/estilar en bloque) |

## viewBox: el sistema de coordenadas

> [!info] viewBox define el "lienzo lógico"
> `viewBox="0 0 100 100"` establece un sistema de coordenadas interno de 100×100 unidades, independiente del tamaño en pantalla. Las formas se dibujan en esas unidades, y el SVG **escala** todo el conjunto al `width`/`height` real. Por eso un SVG con `viewBox` se ve nítido a cualquier tamaño: las coordenadas son relativas, no píxeles fijos.

## Inline vs. como imagen

Hay dos formas de usar SVG, con implicaciones distintas:

| Forma | Cómo | Ventaja |
|-------|------|---------|
| **Inline** (`<svg>` en el HTML) | Pegar el código SVG en el documento | Estilable y animable con CSS/JS; accesible |
| **Como imagen** (`<img src="x.svg">`) | Referenciar un `.svg` externo | Cacheable; aísla el SVG; más simple |

El SVG **inline** permite cambiar colores con CSS (`fill`), animar y responder a eventos; como `<img>` es opaco pero cacheable. Para iconos que cambian de color según el tema, inline; para ilustraciones estáticas reutilizadas, como imagen.

## Estilar y animar con CSS

Al estar en el DOM (inline), las formas se estilan como cualquier elemento:

```css
svg circle { fill: var(--color-marca); transition: fill 0.3s; }
svg:hover circle { fill: #f38ba8; }
```

## Accesibilidad

> [!tip] SVG accesible
> A diferencia del [[01 Lienzo (canvas) | canvas]], el SVG es accesible porque su contenido está en el DOM:
> - Para un SVG con significado, añade `role="img"` y `aria-label` (o un `<title>` como primer hijo).
> - Para un SVG decorativo, `aria-hidden="true"`.
> ```html
> <svg role="img" aria-label="Descarga"> … </svg>
> <svg aria-hidden="true"> <!-- icono decorativo --> </svg>
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa SVG para iconos, logos, ilustraciones y diagramas (escala perfecto, ligero).
> - Inline para estilar/animar con CSS; como `<img>` para ilustraciones estáticas cacheables.
> - Define `viewBox` para que escale; evita medidas fijas en píxeles internas.
> - Añade `<title>`/`aria-label` (significativo) o `aria-hidden` (decorativo).
> - Optimiza el SVG (herramientas como SVGO) para quitar metadatos innecesarios.

## Errores comunes

> [!warning] Trampas
> - **SVG para fotografías**: es vectorial; las fotos son raster (JPEG/WebP).
> - **Sin `viewBox`**: pierde la capacidad de escalar limpiamente.
> - **SVG significativo sin alternativa textual**: inaccesible pese a estar en el DOM.

## Notas relacionadas

- [[01 Lienzo (canvas)]] — la alternativa de píxeles, para muchos objetos.
- [[04 Formatos de Imagen]] — SVG dentro del panorama de formatos.
- [[02 Gráficos Vectoriales (svg)]] — referencia del elemento (esta nota).
