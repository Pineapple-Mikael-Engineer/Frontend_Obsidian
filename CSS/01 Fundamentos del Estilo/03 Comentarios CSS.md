---
title: Comentarios CSS — Documentar y desactivar reglas
aliases:
  - css comments
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# Comentarios CSS

> [!definicion]
> Un **comentario** CSS es texto que el navegador ignora, delimitado por `/* */`. Sirve para documentar las reglas, separar secciones de la hoja y desactivar temporalmente código sin borrarlo.

```css
/* Esto es un comentario */
h1 {
  color: navy;   /* color de marca */
}
```

## Solo existe /* */

> [!warning] CSS no tiene comentarios de una línea
> A diferencia de JavaScript, **CSS no admite `//`** para comentar una línea. El único formato es `/* */`, que puede abarcar una o varias líneas:
> ```css
> /* válido */
> // ❌ esto NO es un comentario en CSS: rompe la regla
> ```
> Escribir `//` deja basura que el navegador intenta parsear y puede invalidar la declaración siguiente.

## Usos

| Uso | Ejemplo |
|-----|---------|
| Documentar | `/* Botón primario */` |
| Separar secciones | `/* === LAYOUT === */` |
| Desactivar código | `/* color: red; */` (la regla deja de aplicarse) |
| Anotar valores | `padding: 1rem; /* = 16px */` |

## Comentarios de sección

En hojas grandes, los comentarios estructuran el archivo y ayudan a orientarse:

```css
/* ==========================================================================
   TIPOGRAFÍA
   ========================================================================== */
body { font-family: system-ui; }

/* ==========================================================================
   COMPONENTES
   ========================================================================== */
.boton { … }
```

## No anidan

> [!warning] Los comentarios no se anidan
> Un `/* */` dentro de otro **no** funciona: el primer `*/` cierra el comentario, y lo que sigue se interpreta como CSS:
> ```css
> /* externo /* interno */ esto ya NO es comentario */
> ```
> Por eso, "comentar" un bloque que ya contiene comentarios puede romperse; cuidado al desactivar secciones.

## Los comentarios viajan al cliente

> [!info] Minificar para producción
> Los comentarios CSS se envían al navegador como parte del archivo: aumentan ligeramente su peso. En producción, las herramientas de **minificación** los eliminan (junto con espacios y saltos) para reducir el tamaño. Por eso se comenta con libertad en desarrollo: el build limpia para producción.

## Buenas prácticas

> [!tip] Recomendaciones
> - Comenta el **porqué** de decisiones no obvias, no lo evidente (`/* color */` sobre `color:` sobra).
> - Usa comentarios de sección para estructurar hojas grandes.
> - Recuerda: solo `/* */`, nunca `//`.
> - Deja que el build minifique; no te prives de comentar en desarrollo.

## Errores comunes

> [!warning] Trampas
> - **Usar `//`**: no es comentario en CSS.
> - **Anidar comentarios**: el primer `*/` cierra todo.
> - **Comentarios obvios** que repiten lo que el código ya dice.

## Notas relacionadas

- [[03 Regla Completa]] — qué se comenta dentro de una hoja.
- [[05 Organización de Archivos/index]] — estructurar hojas grandes.
