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
order: 11
---

# Figura (figure, figcaption)

> [!definicion]
> `<figure>` agrupa un contenido **autocontenido** —imagen, diagrama, fragmento de código, cita,
> tabla, vídeo— que se referencia desde el texto como una unidad y podría moverse a un anexo sin
> romper el flujo. `<figcaption>` le añade una **leyenda** visible y opcional, asociada
> semánticamente a esa figura.

```html
<figure>
  <img src="grafico.png" alt="Ventas por trimestre: crecen cada trimestre de 2026" />
  <figcaption>Fig. 1: Ventas trimestrales en 2026. Fuente: informe anual.</figcaption>
</figure>
```

## No es solo para imágenes

`<figure>` envuelve cualquier contenido referenciable como unidad:

| Contenido | Ejemplo de uso |
|-----------|----------------|
| Imagen | Foto, gráfico, ilustración con su pie |
| Código | Un fragmento numerado al que el texto remite ("ver listado 2") |
| Cita | Una cita destacada con su atribución como leyenda |
| Tabla | Una tabla de datos con su título y fuente |
| Vídeo / diagrama | Un esquema referenciado en la explicación |

## Reglas de figcaption

| Regla | Detalle |
|-------|---------|
| Es opcional | Una figura puede no tener leyenda |
| Posición | Va como **primer** o **último** hijo del `<figure>` |
| Cantidad | Solo **una** `<figcaption>` por figura |

```html
<figure>
  <figcaption>Listado 1: función de saludo.</figcaption>
  <pre><code>function saludar(n){ return `Hola ${n}` }</code></pre>
</figure>
```

## figcaption vs. alt: no son lo mismo

> [!info] Dos textos con funciones distintas
> En una imagen dentro de `<figure>` conviven dos textos que no deben duplicarse:
> - El [[01 Imágenes/01 Imagen (img) | `alt`]] **describe la imagen** para quien no la ve (lectores
>   de pantalla, imagen rota). Responde "¿qué se ve?".
> - La `<figcaption>` es una **leyenda para todos**, que aporta contexto ("Fig. 1", fuente, año).
>
> Ejemplo bien hecho:
> ```html
> <figure>
>   <img src="puente.jpg" alt="Puente colgante rojo sobre una bahía con niebla" />
>   <figcaption>El Golden Gate al amanecer (San Francisco, 1937).</figcaption>
> </figure>
> ```
> El `alt` describe lo visual; la leyenda da el dato. Repetir el mismo texto en ambos es redundante
> para quien usa lector de pantalla.

## Asociación semántica

Envolver imagen + leyenda en `<figure>` **vincula** ambas explícitamente en el árbol de
accesibilidad. Sin `<figure>`, un `<p>` con texto bajo la imagen "se ve" como leyenda pero el
navegador no sabe que lo es: la asociación es solo visual, no semántica.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<figure>` cuando el contenido se **referencia** desde el texto ("ver figura 2") o cuando
>   imagen y pie forman una unidad.
> - Escribe `alt` (qué se ve) y `<figcaption>` (contexto) con funciones distintas, sin duplicar.
> - No envuelvas en `<figure>` una imagen meramente decorativa o de maquetación: reserva el elemento
>   para contenido con valor informativo.

## Errores comunes

> [!warning] Trampas
> - **Duplicar `alt` y `figcaption`** con el mismo texto: redundante en lectores de pantalla.
> - **Dos `<figcaption>`** en una figura: solo se permite una.
> - **Usar `<figure>` para cualquier imagen**: si la imagen es decorativa o inline sin referencia, no
>   necesita figura ni leyenda.

## Notas relacionadas

- [[01 Imágenes/01 Imagen (img)]] — el contenido más común de una figura y su `alt`.
- [[13 Citas en Bloque (blockquote)]] — una cita destacada también puede ir en `<figure>`.
