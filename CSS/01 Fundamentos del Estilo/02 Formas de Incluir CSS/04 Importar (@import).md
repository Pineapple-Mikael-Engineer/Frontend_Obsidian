---
title: "@import — Importar una hoja desde CSS"
aliases:
  - import
  - at-import
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
order: 4
---

# Importar (@import)

> [!definicion]
> La regla `@import` incluye una hoja de estilos **dentro de otra** hoja CSS (o de un `<style>`). Permite trocear el CSS en archivos y juntarlos, pero tiene un **coste de rendimiento** que la hace poco recomendable para producción frente a varios `<link>`.

```css
/* en la hoja principal */
@import "reset.css";
@import "tipografia.css";
@import url("componentes.css");

body { font-family: system-ui; }
```

## Reglas de uso

| Regla | Detalle |
|-------|---------|
| Posición | Debe ir **al principio** de la hoja, antes de cualquier regla de estilo |
| Sintaxis | `@import "ruta";` o `@import url("ruta");` |
| Condición | Admite media queries: `@import "movil.css" (max-width: 600px);` |
| Capas | Puede importar a una `@layer` (ver más abajo) |

## El problema de rendimiento

> [!warning] @import serializa las descargas
> El gran inconveniente de `@import` es que **encadena** las descargas en lugar de paralelizarlas:
> - Con varios `<link>`, el navegador descubre y descarga todas las hojas **en paralelo**.
> - Con `@import`, primero descarga la hoja principal, la parsea, **descubre** el `@import` y **entonces** descarga la siguiente, en serie.
>
> Esto retrasa el render, sobre todo con varios `@import` anidados. Por eso, para enlazar hojas en producción, se prefieren varios `<link>` (o un bundler que las una en una sola).

## Cuándo sí

`@import` tiene usos legítimos:

- **Organización en desarrollo** con un preprocesador (Sass), donde los `@import`/`@use` se resuelven en build a un único archivo (sin coste en runtime).
- **Importar a capas** (`@import "x.css" layer(base)`), una forma cómoda de asignar una hoja entera a una [[03 Capas (@layer)/index | capa de cascada]].
- **Fuentes de un servicio** (`@import url("https://fonts.googleapis.com/...")`), aunque `<link>` suele ser mejor.

## Buenas prácticas

> [!tip] Recomendaciones
> - Para enlazar hojas en producción, usa varios `<link>` (paralelo), no `@import`.
> - Si usas un preprocesador o bundler, los `@import` se resuelven en build: sin coste runtime.
> - `@import ... layer(...)` es una forma limpia de asignar una hoja a una capa.

## Errores comunes

> [!warning] Trampas
> - **`@import` en producción** para trocear CSS: serializa descargas y ralentiza.
> - **Colocarlo tras reglas de estilo**: se ignora (debe ir al inicio).

## Notas relacionadas

- [[01 CSS Externo (link)]] — la alternativa paralela y recomendada.
- [[02 Importar a Capas]] — `@import` con `@layer`.
- [[05 Organización de Archivos/index]] — trocear el CSS de forma mantenible.
