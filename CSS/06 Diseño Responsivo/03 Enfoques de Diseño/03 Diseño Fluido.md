---
title: Diseño Fluido — Adaptación continua sin saltos
aliases:
  - fluid design
  - intrinsic design
tags:
  - css
  - api/concepto
  - responsive
draft: false
---

# Diseño Fluido

> [!definicion]
> El **diseño fluido** (o "intrínseco") hace que la interfaz se adapte **de forma continua** al espacio disponible, sin saltos bruscos en breakpoints. En vez de "móvil → tablet → escritorio" por escalones, los tamaños y el layout **fluyen** suavemente con la pantalla, usando unidades relativas, `clamp()` y rejillas automáticas.

```css
/* Tipografía y espaciado que fluyen sin media queries */
h1 { font-size: clamp(1.75rem, 1rem + 4vw, 3.5rem); }
.galeria { grid-template-columns: repeat(auto-fit, minmax(15rem, 1fr)); }
```

## Las herramientas del diseño fluido

| Herramienta | Hace fluir… |
|-------------|-------------|
| `clamp()` | Tamaños (fuente, espaciado) entre un mín y un máx |
| `auto-fit` + `minmax()` | El número de columnas de una rejilla |
| `%`, `fr`, `flex` | El reparto del espacio |
| `min()`, `max()` | Límites adaptativos |
| Container queries | La adaptación al contenedor |

Detalle: [[02 clamp() para Tipografía Fluida]], [[02 Grid con auto-fit]], [[07 min(), max(), clamp()]] y [[07 Container Queries/index]].

## Menos media queries, más robustez

> [!tip] El diseño fluido reduce los breakpoints
> La gran ventaja: un diseño fluido **necesita menos breakpoints** porque se adapta solo a cualquier ancho intermedio. Donde antes hacían falta tres o cuatro media queries de `font-size`, basta un `clamp()`; donde había varias reglas de columnas, basta un `auto-fit`. Esto hace el diseño **robusto** ante tamaños imprevistos (un plegable, una ventana redimensionada, un tamaño de dispositivo nuevo) sin tener que añadir más cortes.

## Ejemplo: una sección fluida completa

```css
.seccion {
  /* espaciado fluido entre 2rem (móvil) y 6rem (escritorio) */
  padding-block: clamp(2rem, 8vw, 6rem);
  /* ancho fluido con tope */
  width: min(100% - 2rem, 65rem);
  margin-inline: auto;
}
.seccion h2 {
  font-size: clamp(1.5rem, 1rem + 3vw, 3rem);   /* título fluido */
}
.seccion .galeria {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(16rem, 100%), 1fr));
  gap: clamp(1rem, 3vw, 2rem);                   /* gap fluido */
}
```
Esta sección se adapta a **cualquier** ancho sin una sola media query.

## Fluido + breakpoints, no uno u otro

> [!info] La combinación ganadora
> El diseño fluido **no elimina** las media queries: las complementa. La fluidez maneja los ajustes **graduales** (tamaños, columnas), y las media queries quedan para los cambios **estructurales** que no se pueden interpolar:
> - Un menú que pasa de horizontal a **hamburguesa**.
> - Una barra lateral que **aparece** solo en escritorio.
> - Un cambio completo de [[09 Áreas (grid-template-areas) | grid-template-areas]].
>
> Lo gradual fluye; lo estructural salta. Esa división es el enfoque moderno.

## Buenas prácticas

> [!tip] Recomendaciones
> - Haz fluir tamaños con `clamp()` y rejillas con `auto-fit`/`minmax()`.
> - Usa `min(100% - margen, max)` para contenedores fluidos con tope.
> - Reserva las media queries para cambios estructurales (hamburguesa, barra lateral).
> - Incluye una parte en `rem` en los `clamp()` tipográficos (respeta el zoom).

## Errores comunes

> [!warning] Trampas
> - **Demasiados breakpoints** por no aprovechar la fluidez.
> - **`clamp()` solo con `vw`**: ignora el zoom del usuario.
> - **`minmax(15rem, 1fr)` que desborda** en móvil: usa `min(15rem, 100%)`.

## Notas relacionadas

- [[02 clamp() para Tipografía Fluida]] — la tipografía fluida.
- [[02 Grid con auto-fit]] — las rejillas fluidas.
- [[01 Mobile First]] — el enfoque base que se combina con la fluidez.
