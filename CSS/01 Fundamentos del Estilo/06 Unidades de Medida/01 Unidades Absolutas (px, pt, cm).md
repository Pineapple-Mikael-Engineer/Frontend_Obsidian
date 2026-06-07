---
title: Unidades Absolutas — px, pt, cm y la verdad del píxel
aliases:
  - absolute units
  - px
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# Unidades Absolutas (px, pt, cm)

> [!definicion]
> Las unidades **absolutas** representan una longitud **fija**, independiente del contexto. La más usada con diferencia es el **píxel** (`px`); el resto (`pt`, `cm`, `in`…) son herencia del mundo impreso y rara vez se usan en pantalla.

```css
border: 1px solid;
font-size: 16px;
width: 300px;
```

## El catálogo

| Unidad | Equivale a | Uso en pantalla |
|--------|-----------|------------------|
| `px` | El píxel de referencia CSS | El estándar de facto |
| `pt` | 1/72 de pulgada | Impresión (`@media print`) |
| `pc` | 12 pt (pica) | Impresión, raro |
| `in` | 96 px (pulgada) | Impresión |
| `cm` / `mm` | 96px/2.54 por cm | Impresión |
| `Q` | 1/40 de cm | Muy raro |

> [!info] Solo px tiene sentido en pantalla
> Las unidades físicas (`cm`, `in`, `pt`) **no** miden centímetros reales en una pantalla: el navegador las convierte a píxeles CSS con una equivalencia fija (1in = 96px). Por eso un `1cm` en pantalla no mide un centímetro físico. Solo son fiables en **impresión** (`@media print`). En pantalla, usa `px`.

## El píxel CSS no es un píxel físico

> [!info] El píxel de referencia
> Un `px` en CSS **no** equivale necesariamente a un píxel físico de la pantalla. En pantallas de alta densidad (retina, móviles), un píxel CSS abarca varios píxeles físicos (el `devicePixelRatio`). El navegador mantiene el `px` CSS como una unidad de **referencia visual consistente** (~1/96 de pulgada a distancia de lectura típica), de modo que un texto de `16px` se ve de un tamaño parecido en cualquier dispositivo, aunque tenga muchos más píxeles físicos.

## El problema de px para tipografía

> [!warning] px no respeta la preferencia del usuario
> El gran inconveniente de `px` en tamaños de fuente: **no escala** con la configuración de tamaño de texto del navegador. Un usuario con baja visión que sube el tamaño de fuente predeterminado verá crecer el texto en `rem`/`em`, pero **no** el texto fijado en `px`. Esto es una barrera de accesibilidad:
> ```css
> h1 { font-size: 32px; }    /* ❌ ignora la preferencia del usuario */
> h1 { font-size: 2rem; }    /* ✅ escala con ella */
> ```
> Por eso la tipografía y el espaciado se prefieren en [[02 Relativas a Fuente (em, rem, ch, ex) | `rem`]].

## Cuándo px sí es correcto

| Uso de px | Por qué |
|-----------|---------|
| Bordes finos (`1px`, `2px`) | Deben ser nítidos y fijos |
| Radios pequeños | Un `border-radius: 4px` no necesita escalar |
| Sombras sutiles | Desplazamientos precisos |
| Iconos de tamaño fijo | Cuando deben mantenerse exactos |

La regla: `px` para detalles que deben ser **fijos y precisos**; relativo para todo lo que deba **escalar**.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `px` para bordes, radios pequeños y detalles que deban ser exactos.
> - **No** uses `px` para `font-size` ni espaciado principal: usa `rem`.
> - Reserva las unidades físicas (`cm`, `pt`) para `@media print`.
> - Recuerda que el `px` CSS ya gestiona la densidad de pantalla por ti.

## Errores comunes

> [!warning] Trampas
> - **Tipografía en `px`**: ignora la preferencia de tamaño del usuario.
> - **Esperar centímetros reales** de `cm`/`in` en pantalla: no lo son.
> - **Todo en `px`**: produce un diseño rígido que no escala.

## Notas relacionadas

- [[02 Relativas a Fuente (em, rem, ch, ex)]] — la alternativa escalable para tipografía.
- [[05 Porcentajes]] — relativo al contenedor.
- [[index]] — el panorama de unidades.
