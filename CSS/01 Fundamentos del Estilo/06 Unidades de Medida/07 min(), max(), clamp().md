---
title: min(), max(), clamp() — Acotar valores
aliases:
  - min max clamp
  - clamp
tags:
  - css
  - api/funcion
  - responsive
draft: false
---

# min(), max(), clamp()

> [!definicion]
> Estas tres funciones **acotan** un valor entre límites, sin media queries. `min()` toma el menor de varios valores (impone un **máximo**), `max()` el mayor (impone un **mínimo**), y `clamp()` fija un **rango** con mínimo, valor preferido y máximo. Son el corazón del diseño **fluido** moderno.

```css
width: min(90%, 60rem);            /* nunca más de 60rem */
font-size: max(1rem, 2.5vw);       /* nunca menor de 1rem */
font-size: clamp(1rem, 2.5vw, 2rem);  /* entre 1rem y 2rem, fluido en medio */
```

## Las tres funciones

| Función | Devuelve | Efecto práctico |
|---------|----------|------------------|
| `min(a, b, …)` | El valor **más pequeño** | Impone un **techo** (máximo) |
| `max(a, b, …)` | El valor **más grande** | Impone un **suelo** (mínimo) |
| `clamp(min, val, max)` | `val` acotado a `[min, max]` | Un **rango** completo |

> [!info] La paradoja de min/max
> Resulta contraintuitivo: **`min()` impone un máximo** y **`max()` impone un mínimo**. La clave: `min(90%, 60rem)` elige el menor, así que nunca supera `60rem` → `60rem` actúa de **tope**. `max(1rem, 2.5vw)` elige el mayor, así que nunca baja de `1rem` → `1rem` actúa de **suelo**. Se entiende pensando en qué valor "gana" en cada extremo.

## clamp(): el rango fluido

`clamp(MIN, PREFERIDO, MAX)` es el más usado: el valor `PREFERIDO` (normalmente fluido, con `vw` o `%`) se usa mientras quede dentro de `[MIN, MAX]`; fuera, se recorta al límite:

```css
/* Tipografía fluida: 1.5rem en móvil, hasta 3rem en escritorio */
h1 { font-size: clamp(1.5rem, 1rem + 4vw, 3rem); }
```

Esto reemplaza varias media queries de `font-size` con **una** línea que escala suavemente.

## Patrones habituales

```css
/* Contenedor centrado, fluido pero con ancho máximo */
.contenedor { width: min(100% - 2rem, 65rem); margin-inline: auto; }

/* Espaciado fluido entre secciones */
section { padding-block: clamp(2rem, 8vw, 6rem); }

/* Tamaño de fuente que nunca baja de lo legible */
small { font-size: max(0.75rem, 0.7em); }
```

## El término preferido con calc implícito

> [!tip] Suma dentro de clamp sin calc()
> Dentro de `min`/`max`/`clamp`, puedes mezclar unidades **sin** envolver en `calc()` (estas funciones ya admiten aritmética):
> ```css
> font-size: clamp(1rem, 1rem + 2vw, 2rem);   /* la suma va directa */
> ```
> Combinar una base en `rem` con un término en `vw` (`1rem + 2vw`) da una curva de escalado más natural que solo `vw`, y respeta mejor el zoom del usuario.

## Accesibilidad del zoom

> [!warning] Incluye una unidad relativa en clamp tipográfico
> Un `clamp()` de tipografía que use **solo** `vw` en el término preferido (`clamp(1rem, 4vw, 2rem)`) puede no responder bien al **zoom de texto** del usuario. Incluir una parte en `rem` (`1rem + 2vw`) mantiene la escalabilidad accesible: el texto sigue creciendo si el usuario aumenta la fuente.

## Buenas prácticas

> [!tip] Recomendaciones
> - `clamp()` para tipografía y espaciado fluidos: sustituye media queries.
> - `min(100% - 2rem, 65rem)` para contenedores fluidos con tope.
> - En `clamp()` tipográfico, combina `rem + vw` para respetar el zoom.
> - Recuerda: `min()` pone tope, `max()` pone suelo.

## Errores comunes

> [!warning] Trampas
> - **Confundir `min`/`max`**: dan el contrario de lo que el nombre sugiere.
> - **`clamp()` solo con `vw`**: puede ignorar el zoom de texto.
> - **Orden incorrecto en `clamp`**: si `MIN` > `MAX`, gana `MIN`.

## Notas relacionadas

- [[06 calc()]] — cálculo de un único valor.
- [[02 clamp() para Tipografía Fluida]] — `clamp()` aplicado a fuentes.
- [[03 Relativas al Viewport (vw, vh, vmin, vmax)]] — las unidades fluidas que se acotan.
