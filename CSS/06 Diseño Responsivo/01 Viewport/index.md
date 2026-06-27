---
title: Viewport — El área visible y su configuración
aliases:
  - viewport
tags:
  - css
  - api/concepto
  - responsive
draft: false
order: 1
---

# Viewport

> [!definicion]
> El **viewport** es el área visible de la página en el navegador. En móvil, su configuración mediante la etiqueta [[01 meta viewport | `<meta viewport>`]] es el **prerrequisito** de todo diseño responsivo: sin ella, el móvil simula una pantalla de escritorio y reduce la página, ignorando las media queries.

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

## Mapa de la subsección

- [[01 meta viewport]] — la etiqueta imprescindible y sus directivas.
- [[02 viewport-fit]] — cubrir la zona del notch en móviles con muesca.
- [[03 interactive-widget]] — cómo el teclado virtual afecta al viewport.

## Por qué es el cimiento

> [!warning] Sin la etiqueta, no hay responsive
> El error que invalida todo lo demás: olvidar `<meta viewport>`. Sin ella, el navegador móvil asume un viewport de ~980px y **escala** la página para que quepa, mostrándola diminuta. Las media queries se comparan contra ese 980px ficticio, así que `@media (max-width: 600px)` **nunca** se activa en el móvil. La etiqueta `width=device-width` iguala el viewport al ancho real del dispositivo, y ahí empieza el responsive de verdad. Detalle (también desde HTML) en [[02 Viewport (meta viewport) | la nota HTML]].

## Notas relacionadas

- [[01 meta viewport]] — el punto de partida.
- [[02 Viewport (meta viewport)]] — la etiqueta desde el curso de HTML.
- [[04 Viewport Dinámico (svh, lvh, dvh)]] — las unidades de viewport que resuelven el "100vh en móvil".
