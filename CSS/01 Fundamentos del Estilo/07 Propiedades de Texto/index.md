---
title: Propiedades de Texto — Estilar la tipografía básica
aliases:
  - text properties
tags:
  - css
  - api/concepto
  - tipografia
draft: false
---

# Propiedades de Texto

> [!definicion]
> Un conjunto de propiedades controla cómo se ve el **texto**: su color, su fuente, su tamaño, su peso, su alineación y su espaciado. La mayoría **se heredan**, así que aplicarlas a un contenedor (o al `body`) las propaga a todo el texto interior, lo que las hace ideales para los estilos base.

```css
body {
  color: #1e1e2e;
  font-family: system-ui, sans-serif;
  font-size: 1rem;
  line-height: 1.6;
}
```

## Mapa de la sección

| Propiedad | Controla | Hereda |
|-----------|----------|--------|
| `color` | Color del texto | Sí |
| `font-family` | La tipografía | Sí |
| `font-size` | El tamaño | Sí |
| `font-weight` | El grosor | Sí |
| `font-style` | Cursiva/normal | Sí |
| `font-variant` | Versalitas y variantes | Sí |
| `line-height` | Alto de línea (interlineado) | Sí |
| `text-align` | Alineación horizontal | Sí |
| `text-decoration` | Subrayado, tachado | No (parcial) |
| `text-transform` | Mayúsculas/minúsculas | Sí |
| `text-indent` | Sangría de primera línea | Sí |
| `letter-spacing`/`word-spacing` | Espaciado entre letras/palabras | Sí |
| `white-space` | Manejo de espacios y saltos | Sí |

Cada una en su nota: [[01 color]], [[02 font-family]], [[03 font-size]], [[04 font-weight]], [[05 font-style]], [[06 font-variant]], [[07 line-height]], [[08 text-align]], [[09 text-decoration]], [[10 text-transform]], [[11 text-indent]], [[12 letter-spacing y word-spacing]] y [[13 white-space]].

## La herencia como aliada

> [!tip] Estilos de texto en el body
> Como casi todas estas propiedades **se heredan**, el patrón estándar es declararlas una vez en `body` (o `:root`) para fijar la tipografía base de todo el sitio, y luego sobrescribir solo donde haga falta:
> ```css
> body { font-family: system-ui; font-size: 1rem; line-height: 1.6; color: #1e1e2e; }
> h1   { font-size: 2.5rem; }   /* solo lo que cambia */
> ```
> El detalle de la herencia en [[01 Herencia Natural]].

## El shorthand font

Varias de estas propiedades tienen un atajo, `font`, que las agrupa (con un orden obligatorio):

```css
font: italic bold 1.2rem/1.6 Georgia, serif;
/*     style  weight size/line-height  family */
```

Es compacto pero exige recordar el orden y **resetea** las propiedades no incluidas, por lo que muchos prefieren declararlas por separado.

## Notas relacionadas

- [[03 font-size]] · [[07 line-height]] — el dúo que define la legibilidad.
- [[03 Tipografía Avanzada/index]] — fuentes web, ligaduras, decoración avanzada.
- [[01 Herencia Natural]] — por qué estas propiedades se propagan.
