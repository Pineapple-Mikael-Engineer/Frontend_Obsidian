---
title: Textos Alternativos — Alternativas para contenido no textual
aliases:
  - text alternatives
  - alt text
tags:
  - html
  - api/concepto
  - a11y
draft: false
order: 3
---

# Textos Alternativos

> [!definicion]
> El contenido **no textual** (imágenes, iconos, gráficos) necesita una **alternativa textual** para quien no puede percibirlo. Es uno de los requisitos más básicos y conocidos de la accesibilidad (WCAG 1.1.1): cada imagen con información necesita su `alt`, cada icono su nombre, cada gráfico su descripción.

```html
<img src="grafico.png" alt="Las ventas crecen cada trimestre de 2026" />
<button aria-label="Buscar"><svg aria-hidden="true">…</svg></button>
```

## Mapa de la sección

- [[01 alt en Imágenes]] — escribir buenos textos alternativos para `<img>`.
- [[02 Texto para Iconos]] — dar nombre accesible a iconos y botones de icono.
- [[03 Contenido Oculto Visualmente (sr-only)]] — texto solo para lectores de pantalla.

## El principio: ¿qué información transmite?

> [!info] La alternativa depende de la función, no de la imagen
> El texto alternativo no describe "qué se ve" en abstracto, sino **qué información o función** aporta el elemento en su contexto:
> - Una foto informativa → describe su contenido relevante.
> - Un icono de "guardar" → describe la **acción** ("Guardar"), no el dibujo del disquete.
> - Un adorno decorativo → alternativa **vacía** (`alt=""`), para que se ignore.
> - Un gráfico complejo → resumen + descripción larga.
>
> La misma imagen puede necesitar alternativas distintas según para qué se use.

## Decoración vs. información

| Tipo | Alternativa |
|------|-------------|
| Aporta información | Texto descriptivo |
| Es un control (botón/enlace) | Describe la acción/destino |
| Es puramente decorativo | Alternativa **vacía** (`alt=""` / `aria-hidden`) |

Marcar lo decorativo como vacío es tan importante como describir lo informativo: evita que el lector lea ruido (nombres de archivo, descripciones inútiles).

## Notas relacionadas

- [[01 alt en Imágenes]] — el caso central.
- [[01 Imagen (img)]] — el atributo `alt` desde el elemento `<img>`.
- [[02 Gráficos Vectoriales (svg)]] — alternativas para SVG.
