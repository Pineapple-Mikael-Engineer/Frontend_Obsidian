---
title: clamp() para Tipografía Fluida — El método recomendado
aliases:
  - fluid type clamp
tags:
  - css
  - api/concepto
  - responsive
draft: false
order: 2
---

# clamp() para Tipografía Fluida

> [!definicion]
> `clamp(MIN, PREFERIDO, MAX)` es la forma **recomendada** de hacer tipografía fluida: el texto escala con la pantalla (término `PREFERIDO`, con `vw`) pero **acotado** entre un mínimo y un máximo. Reemplaza varias media queries de `font-size` por una sola línea robusta y accesible.

```css
h1 { font-size: clamp(1.75rem, 1rem + 4vw, 3.5rem); }
```

## La anatomía del valor

```css
font-size: clamp(MIN, PREFERIDO, MAX);
```

| Parte | Ejemplo | Función |
|-------|---------|---------|
| `MIN` | `1.75rem` | El tamaño mínimo (en móvil) |
| `PREFERIDO` | `1rem + 4vw` | El tamaño fluido (escala con la ventana) |
| `MAX` | `3.5rem` | El tamaño máximo (en pantallas grandes) |

El navegador usa `PREFERIDO` mientras esté entre `MIN` y `MAX`; fuera, se recorta al límite.

## El término preferido: rem + vw

> [!tip] Combina una base rem con un poco de vw
> La clave de un buen `clamp()` tipográfico está en el término del medio. **No** uses solo `vw`: combina una base en `rem` (que respeta el zoom) con un componente en `vw` (la fluidez):
> ```css
> font-size: clamp(1.75rem, 1rem + 4vw, 3.5rem);
> /*                 mín     base+fluido    máx */
> ```
> - El `1rem` da una base que **responde al zoom del usuario** (accesibilidad).
> - El `4vw` aporta el crecimiento con la pantalla.
> - El `MIN`/`MAX` evitan los extremos.

## Por qué reemplaza varias media queries

> [!info] Una línea en lugar de tres breakpoints
> Sin `clamp()`, escalar un título requería varias media queries:
> ```css
> /* ❌ El método antiguo */
> h1 { font-size: 1.75rem; }
> @media (min-width: 600px)  { h1 { font-size: 2.5rem; } }
> @media (min-width: 1000px) { h1 { font-size: 3.5rem; } }
> ```
> Con `clamp()`, **una línea** hace lo mismo y mejor (transición continua, sin saltos):
> ```css
> h1 { font-size: clamp(1.75rem, 1rem + 4vw, 3.5rem); }
> ```

## Una escala fluida completa

> [!tip] clamp() en toda la escala tipográfica
> Se puede aplicar `clamp()` a todos los niveles de la escala, definidos en variables:
> ```css
> :root {
>   --txt-base: clamp(1rem, 0.95rem + 0.3vw, 1.125rem);
>   --txt-lg:   clamp(1.25rem, 1.1rem + 0.8vw, 1.5rem);
>   --txt-h2:   clamp(1.5rem, 1rem + 2.5vw, 2.5rem);
>   --txt-h1:   clamp(1.75rem, 1rem + 4vw, 3.5rem);
> }
> h1 { font-size: var(--txt-h1); }
> ```
> Toda la tipografía fluye de forma coherente, sin media queries. Herramientas online ("fluid type scale") calculan los valores a partir de los tamaños deseados en dos breakpoints.

## La accesibilidad del zoom

> [!warning] El rem en el preferido es obligatorio para el zoom
> Si el término preferido fuera **solo** `vw` (`clamp(1.75rem, 5vw, 3.5rem)`), el texto podría no responder bien al **zoom de texto** del usuario, fallando un criterio de accesibilidad. Incluir una parte en `rem` (`1rem + 4vw`) asegura que el texto siga creciendo cuando el usuario aumenta el tamaño de fuente del navegador.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `clamp(MIN, <rem> + <vw>, MAX)` para tipografía fluida.
> - **Siempre** una base en `rem` en el término preferido (respeta el zoom).
> - Define la escala en variables para coherencia.
> - Usa una calculadora de "fluid type" para obtener los valores de dos breakpoints.

## Errores comunes

> [!warning] Trampas
> - **Solo `vw`** en el preferido: ignora el zoom (accesibilidad).
> - **MIN > MAX**: gana el MIN; revisa el orden.
> - **Rango demasiado amplio**: el texto crece de forma poco controlada en el medio.

## Notas relacionadas

- [[01 Unidades vw para Fuente]] — por qué `vw` solo no basta.
- [[07 min(), max(), clamp()]] — la función `clamp()` en detalle.
- [[03 Diseño Fluido]] — la filosofía que `clamp()` encarna.
