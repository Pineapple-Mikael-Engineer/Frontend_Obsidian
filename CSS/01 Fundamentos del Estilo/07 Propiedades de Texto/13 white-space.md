---
title: white-space — Manejo de espacios y saltos de línea
aliases:
  - white-space
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: white-space
grupo: tipografia
valor_inicial: normal
hereda: true
animable: false
draft: false
---

# white-space

> [!definicion]
> `white-space` controla cómo se tratan los **espacios en blanco** (espacios, tabulaciones, saltos de línea) del HTML y si el texto **ajusta** (hace wrap) al llegar al borde. Por defecto, el HTML colapsa los espacios consecutivos en uno y ajusta el texto; `white-space` permite cambiar ese comportamiento.

```css
.codigo  { white-space: pre; }       /* conserva espacios y saltos */
.una-linea { white-space: nowrap; }  /* nunca salta de línea */
```

## Valores

| Valor | Colapsa espacios | Respeta saltos del HTML | ¿Ajusta (wrap)? |
|-------|------------------|--------------------------|-----------------|
| `normal` | Sí | No | Sí |
| `nowrap` | Sí | No | **No** |
| `pre` | **No** | **Sí** | No |
| `pre-wrap` | **No** | **Sí** | Sí |
| `pre-line` | Sí | **Sí** | Sí |
| `break-spaces` | No | Sí | Sí (incluye espacios) |

## Los tres comportamientos que combina

> [!info] Tres decisiones en una propiedad
> Cada valor mezcla tres reglas:
> 1. **¿Colapsa** los espacios/tabs consecutivos en uno?
> 2. **¿Respeta** los saltos de línea del código fuente?
> 3. **¿Ajusta** el texto al ancho del contenedor (wrap)?
>
> Por eso hay tantos valores: son las combinaciones útiles de esas tres decisiones.

## Los casos prácticos

```css
/* Código: respetar todo, no ajustar (con scroll si hace falta) */
pre { white-space: pre; overflow-x: auto; }

/* Texto que no debe partirse (un botón, una etiqueta) */
.boton { white-space: nowrap; }

/* Conservar saltos del usuario pero permitir ajuste (comentarios) */
.comentario { white-space: pre-wrap; }
```

## nowrap y el desbordamiento

> [!warning] nowrap puede desbordar
> `white-space: nowrap` impide el salto de línea, así que un texto largo **se sale** del contenedor (o lo ensancha). Suele combinarse con `overflow: hidden` y [[05 text-overflow | `text-overflow: ellipsis`]] para recortar con puntos suspensivos:
> ```css
> .titulo-corto {
>   white-space: nowrap;
>   overflow: hidden;
>   text-overflow: ellipsis;
> }
> ```

## pre-wrap: el más útil para contenido de usuario

`pre-wrap` es ideal para mostrar texto que el usuario escribió con saltos de línea (un comentario, un mensaje): **conserva** sus saltos y espacios pero **permite** que las líneas largas se ajusten al contenedor, evitando el scroll horizontal de `pre`.

## Buenas prácticas

> [!tip] Recomendaciones
> - `pre` (o `<pre>` HTML) para código; con `overflow-x: auto` para líneas largas.
> - `pre-wrap` para texto de usuario con saltos propios.
> - `nowrap` + `overflow` + `text-overflow: ellipsis` para recortar texto en una línea.
> - Recuerda que se hereda: aplicarlo a un contenedor afecta a su texto.

## Errores comunes

> [!warning] Trampas
> - **`nowrap` sin control de overflow**: el texto desborda el layout.
> - **`pre` para texto de usuario**: causa scroll horizontal; usa `pre-wrap`.
> - **Confundir los valores**: revisa la tabla de las tres decisiones.

## Notas relacionadas

- [[05 text-overflow]] — recortar con puntos suspensivos (necesita `nowrap`).
- [[01 overflow-wrap y word-wrap]] — partir palabras largas que no caben.
- [[18 Código Preformateado (pre)]] — el elemento `<pre>`, que usa `white-space: pre`.
