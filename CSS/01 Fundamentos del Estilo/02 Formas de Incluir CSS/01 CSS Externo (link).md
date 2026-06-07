---
title: CSS Externo (link) — Hoja de estilos en archivo aparte
aliases:
  - external css
  - link stylesheet
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# CSS Externo (link)

> [!definicion]
> El CSS **externo** vive en un archivo `.css` separado, enlazado al HTML con `<link rel="stylesheet" href="…">`. Es la forma **recomendada** en producción: separa presentación de estructura, se cachea entre páginas y centraliza el mantenimiento.

```html
<head>
  <link rel="stylesheet" href="/css/estilos.css" />
</head>
```

```css
/* estilos.css */
body { font-family: system-ui; }
h1 { color: #cba6f7; }
```

## Ventajas

| Ventaja | Por qué |
|---------|---------|
| Cacheable | El navegador descarga el `.css` una vez y lo reutiliza en todas las páginas |
| Reutilizable | Un archivo sirve a todo el sitio |
| Mantenible | Un solo lugar que tocar; separado del HTML |
| Paralelizable | El navegador puede descargarlo en paralelo |

## Varias hojas y el orden

Se pueden enlazar varias hojas; se aplican en el **orden** en que aparecen (relevante para la [[02 Cascada/index | cascada]]):

```html
<link rel="stylesheet" href="reset.css" />
<link rel="stylesheet" href="base.css" />
<link rel="stylesheet" href="componentes.css" />
```

Una regla de `componentes.css` que compita con una de `base.css` (misma especificidad) gana, por ir **después**.

## media: hojas condicionales

El atributo `media` aplica una hoja solo bajo cierta condición, descargándola con baja prioridad si no se cumple:

```html
<link rel="stylesheet" href="print.css" media="print" />
<link rel="stylesheet" href="ancho.css" media="(min-width: 1024px)" />
```

## Rendimiento: render-blocking

> [!info] El CSS bloquea el primer pintado (y está bien)
> El CSS externo en el `<head>` es **render-blocking**: el navegador no pinta hasta tenerlo, lo que evita el destello de contenido sin estilo (FOUC). La contrapartida: una hoja enorme retrasa el pintado. Técnicas como el CSS crítico inline (con [[02 CSS Interno (style) | `<style>`]]) y diferir el resto mitigan esto. El detalle de carga está en la nota HTML de [[07 Enlace a CSS (link) | `<link>`]].

## Buenas prácticas

> [!tip] Recomendaciones
> - CSS externo por defecto en producción.
> - Ordena las hojas según la cascada que quieras (reset → base → componentes).
> - Usa `media` para hojas condicionales (impresión, breakpoints).
> - Para CDN externos, añade `integrity` + `crossorigin` (SRI).

## Errores comunes

> [!warning] Trampas
> - **Olvidar `rel="stylesheet"`**: el navegador no aplica el CSS.
> - **Ruta mal calculada**: `href` se resuelve respecto a la URL del documento.
> - **Demasiadas hojas pequeñas**: cada una es una petición (menos crítico con HTTP/2).

## Notas relacionadas

- [[02 CSS Interno (style)]] — CSS embebido, para casos puntuales.
- [[04 Importar (@import)]] — importar hojas desde CSS (con cautela).
- [[07 Enlace a CSS (link)]] — el elemento `<link>` desde HTML.
