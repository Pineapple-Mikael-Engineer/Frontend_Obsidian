---
title: <figure> y <figcaption> — Contenido referenciado con leyenda
aliases:
  - figure element
  - figcaption
tags:
  - html
  - api/elemento
  - semantica
elemento: figure
categoria: flujo
rol_implicito: figure
vacio: false
draft: false
---

# Figura (figure, figcaption)

> [!definicion]
> `<figure>` agrupa un contenido **autocontenido** —imagen, diagrama, fragmento de código, cita,
> tabla— que se referencia desde el texto como una unidad. `<figcaption>` le añade una **leyenda**
> opcional. La figura podría moverse a un anexo sin romper el flujo.

```html
<figure>
  <img src="grafico.png" alt="Ventas trimestrales 2026" />
  <figcaption>Fig. 1: Ventas por trimestre en 2026.</figcaption>
</figure>
```

## Reglas

| Regla | Detalle |
|-------|---------|
| `<figcaption>` es opcional | Si está, va como **primer o último** hijo del `<figure>` |
| Una sola `<figcaption>` | Solo una leyenda por figura |
| El contenido no es solo imágenes | También código, citas, vídeos, tablas |

> [!info] alt y figcaption no son lo mismo
> En una imagen, el [[01 Imágenes/01 Imagen (img) | `alt`]] describe la imagen para quien **no la
> ve**; la `<figcaption>` es una leyenda visible para **todos**. No deben duplicarse: el `alt`
> describe el contenido visual, la leyenda aporta contexto ("Fig. 1", fuente, año).

> [!tip] Asociación semántica
> Envolver imagen + leyenda en `<figure>` las vincula explícitamente. Sin `<figure>`, un `<p>` con
> el texto bajo la imagen "se ve" como leyenda pero el navegador no sabe que lo es.

## Notas relacionadas

- [[01 Imágenes/01 Imagen (img)]] — el contenido más común de una figura.
- [[13 Citas en Bloque (blockquote)]] — una cita larga también puede ir en `<figure>`.
