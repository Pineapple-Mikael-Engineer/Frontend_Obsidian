---
title: font-variation-settings — Los ejes de las fuentes variables
aliases:
  - font-variation-settings
  - variable fonts
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: font-variation-settings
grupo: tipografia
valor_inicial: normal
hereda: true
animable: true
draft: false
order: 3
---

# font-variation-settings

> [!definicion]
> `font-variation-settings` controla los **ejes** de una **fuente variable**: un único archivo que contiene un rango continuo de variaciones (peso, ancho, inclinación, tamaño óptico…) en vez de archivos separados por estilo. Permite valores intermedios exactos (`peso 437`) imposibles con fuentes estáticas.

```css
h1 {
  font-variation-settings: "wght" 625, "wdth" 110;
}
```

## Qué es una fuente variable

> [!info] Un archivo, infinitos estilos
> Una fuente **estática** necesita un archivo por estilo (Regular, Bold, Light, Italic…). Una fuente **variable** mete todo en **un solo archivo** con uno o varios **ejes** continuos: en vez de "Regular o Bold", tienes cualquier peso entre 100 y 900. Ventajas:
> - **Un archivo** en lugar de muchos (menos descargas).
> - **Cualquier valor** intermedio del eje.
> - **Animable**: puedes transicionar suavemente el peso o el ancho.

## Los ejes estándar (registrados)

| Eje | Etiqueta | Controla | Atajo de alto nivel |
|-----|----------|----------|---------------------|
| Peso | `wght` | Grosor (100–900) | `font-weight` |
| Ancho | `wdth` | Condensado/expandido | `font-width` / `font-stretch` |
| Inclinación | `slnt` | Ángulo oblicuo | `font-style: oblique <ángulo>` |
| Cursiva | `ital` | Normal/italic | `font-style` |
| Tamaño óptico | `opsz` | Ajuste según tamaño | `font-optical-sizing` |

Las fuentes también pueden definir **ejes personalizados** (mayúsculas: `GRAD`, `CASL`…), sin atajo, accesibles solo por `font-variation-settings`.

## Prefiere los atajos para los ejes estándar

> [!tip] font-weight ya mueve el eje wght
> Para los ejes **registrados**, usa las propiedades de alto nivel, no `font-variation-settings`:
> ```css
> /* ✅ legible, y la fuente variable lo respeta */
> h1 { font-weight: 625; }
> /* ❌ innecesariamente verboso para lo mismo */
> h1 { font-variation-settings: "wght" 625; }
> ```
> `font-weight`, `font-style` y `font-stretch` ya mueven sus ejes en una fuente variable. Reserva `font-variation-settings` para **ejes personalizados** (`GRAD`, etc.) que no tienen propiedad propia.

## Animación de ejes

> [!tip] Animar el peso suavemente
> Como los ejes son continuos, se pueden **animar**: un texto que engorda al pasar el ratón, un logo que "respira". Con fuentes estáticas esto sería un salto brusco; con variables, una transición fluida:
> ```css
> h1 { transition: font-weight 0.3s; }
> h1:hover { font-weight: 800; }
> ```

## No es acumulativa

Como [[02 font-feature-settings | `font-feature-settings`]], `font-variation-settings` **reemplaza** todos los ejes en cada declaración: hay que listar todos juntos. Otra razón para usar los atajos cuando existan.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa fuentes variables para reducir descargas y ganar flexibilidad de peso/ancho.
> - Controla los ejes estándar con `font-weight`/`font-style`/`font-stretch`, no con el descriptor de bajo nivel.
> - Reserva `font-variation-settings` para ejes personalizados de la fuente.
> - Aprovecha que los ejes son animables para microinteracciones.

## Errores comunes

> [!warning] Trampas
> - **`font-variation-settings: "wght"`** donde `font-weight` bastaría.
> - **Declaraciones que se pisan** (no es acumulativa): lista todos los ejes.
> - **Usar valores fuera del rango** que la fuente define para el eje.

## Notas relacionadas

- [[04 font-weight]] — el eje de peso vía atajo.
- [[04 font-optical-sizing]] — el eje de tamaño óptico.
- [[01 Fuentes Web (@font-face)/index]] — cargar una fuente variable.
