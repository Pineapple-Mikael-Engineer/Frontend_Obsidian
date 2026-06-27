---
title: Propiedad content — Qué generan before y after
aliases:
  - content
tags:
  - css
  - api/propiedad
  - selectores
propiedad: content
grupo: pseudoelemento
valor_inicial: normal
hereda: false
animable: false
draft: false
order: 2
---

# Propiedad content

> [!definicion]
> `content` define **qué** insertan los pseudoelementos [[01 before y after | `::before`/`::after`]]: texto literal, el valor de un atributo (`attr()`), comillas, una imagen, un contador, o nada (`""`). Es la propiedad que da existencia y contenido a los pseudoelementos generados.

```css
.externo::after { content: " (externo)"; }
.tooltip::before { content: attr(data-tip); }
```

## Los valores de content

| Valor | Genera |
|-------|--------|
| `"texto"` | Texto literal |
| `""` | Vacío (para formas decorativas) |
| `attr(data-x)` | El valor de un atributo del elemento |
| `open-quote` / `close-quote` | Comillas según el idioma |
| `url(icono.svg)` | Una imagen |
| `counter(nombre)` | Un contador CSS |
| `none` / `normal` | Nada (el pseudoelemento no se genera) |

## Texto literal y vacío

```css
.requerido::after { content: " *"; color: red; }   /* texto */
.barra::before { content: ""; display: block; height: 2px; }   /* vacío decorativo */
```

## attr(): leer atributos del HTML

> [!tip] Mostrar el valor de un atributo
> `attr()` inserta el valor de un atributo del elemento, útil para tooltips, etiquetas y depuración:
> ```css
> /* Un tooltip desde un atributo data */
> [data-tooltip]:hover::after {
>   content: attr(data-tooltip);
>   position: absolute;
>   background: #333; color: white; padding: 0.25rem 0.5rem;
> }
> /* Mostrar la URL de los enlaces al imprimir */
> @media print {
>   a::after { content: " (" attr(href) ")"; }
> }
> ```
> `attr()` se concatena con texto literal poniéndolos seguidos: `content: "(" attr(href) ")"`.

## Comillas tipográficas

> [!tip] open-quote respeta el idioma
> `open-quote` y `close-quote` insertan las comillas correctas según el idioma (definidas por la propiedad `quotes` y la pseudoclase [[05 lang() | `:lang()`]]):
> ```css
> blockquote::before { content: open-quote; }
> blockquote::after  { content: close-quote; }
> :lang(es) { quotes: "«" "»"; }
> ```

## Imágenes y contadores

```css
/* Un icono como imagen */
.check::before { content: url("check.svg"); }

/* Numeración personalizada con contadores */
ol { counter-reset: item; }
li::before { counter-increment: item; content: counter(item) ". "; }
```

## content con texto alternativo (accesibilidad)

> [!tip] La sintaxis content / "texto alt"
> Para imágenes/iconos generados, la sintaxis moderna permite dar un **texto alternativo** para lectores de pantalla, mejorando la accesibilidad del contenido generado:
> ```css
> .icono-info::before {
>   content: url("info.svg") / "Información";   /* imagen / texto alt */
> }
> ```
> La parte tras la barra es lo que anuncia la asistencia, resolviendo en parte el problema de accesibilidad de los pseudoelementos.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `attr()` para tooltips, etiquetas y mostrar URLs al imprimir.
> - `open-quote`/`close-quote` + `quotes` + `:lang()` para comillas correctas.
> - `content: ""` para formas decorativas (recuerda que es obligatorio).
> - Usa la sintaxis `content: url() / "alt"` para dar texto alternativo accesible.

## Errores comunes

> [!warning] Trampas
> - **Olvidar `content`**: el pseudoelemento no aparece.
> - **Información esencial** en `content`: poco fiable para la accesibilidad (salvo con `/ "alt"`).
> - **Concatenación mal escrita**: `attr()` y texto van seguidos, sin comas.

## Notas relacionadas

- [[01 before y after]] — los pseudoelementos que usan `content`.
- [[05 lang()]] — el idioma para las comillas.
- [[12 Atributos de Datos (data-*)]] — los atributos que lee `attr()`.
