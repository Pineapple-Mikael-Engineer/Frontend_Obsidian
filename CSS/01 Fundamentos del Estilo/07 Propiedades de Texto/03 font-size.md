---
title: font-size — Tamaño del texto
aliases:
  - font-size
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: font-size
grupo: tipografia
valor_inicial: medium
hereda: true
animable: true
draft: false
order: 3
---

# font-size

> [!definicion]
> `font-size` define el **tamaño** del texto. Es una de las propiedades más fundamentales: además de dimensionar las letras, establece la referencia de la unidad `em` y del `line-height` relativo del propio elemento.

```css
h1   { font-size: 2.5rem; }
body { font-size: 1rem; }
small { font-size: 0.875rem; }
```

## Tipos de valor

| Valor | Ejemplo | Comportamiento |
|-------|---------|----------------|
| Longitud absoluta | `16px` | Fijo; **no escala** con la preferencia del usuario |
| Relativa a raíz | `1rem` | Escala con la fuente raíz (recomendado) |
| Relativa al padre | `1.2em` / `120%` | Multiplica el tamaño del padre (se acumula al anidar) |
| Palabra clave absoluta | `small`, `medium`, `large` | Tamaños predefinidos |
| Palabra clave relativa | `smaller`, `larger` | Respecto al padre |
| Fluida | `clamp(1rem, 2vw, 2rem)` | Escala con la ventana, acotada |

## rem para accesibilidad

> [!warning] Evita px en font-size
> Un `font-size` en `px` **no responde** a la configuración de tamaño de texto del navegador, una barrera para usuarios con baja visión. `rem` sí escala con ella:
> ```css
> h1 { font-size: 32px; }    /* ❌ ignora la preferencia */
> h1 { font-size: 2rem; }    /* ✅ escala (32px si la raíz es 16px) */
> ```
> Detalle de las unidades en [[02 Relativas a Fuente (em, rem, ch, ex) | rem/em]].

## em se acumula en cascada

`font-size: 1.2em` significa "1.2 × el tamaño del padre". Esto es útil para componentes que escalan, pero **se compone** al anidar: dos niveles con `1.2em` dan 1.44×. Por eso `rem` (relativa a la raíz, no al padre) es más predecible para `font-size`.

## Tipografía fluida con clamp()

Para que el tamaño crezca con la pantalla sin media queries, se usa [[07 min(), max(), clamp() | `clamp()`]]:

```css
h1 { font-size: clamp(1.75rem, 1rem + 3vw, 3rem); }
```

Incluir una parte en `rem` (no solo `vw`) mantiene la respuesta al zoom del usuario.

## Escalas tipográficas

> [!tip] Usa una escala, no tamaños arbitrarios
> En vez de elegir tamaños al azar, las interfaces coherentes usan una **escala tipográfica** (una progresión: 1rem, 1.25rem, 1.5rem, 2rem, 2.5rem…), a menudo guardada en variables:
> ```css
> :root {
>   --txt-sm: 0.875rem; --txt-base: 1rem;
>   --txt-lg: 1.25rem; --txt-xl: 1.5rem; --txt-2xl: 2rem;
> }
> ```
> Da ritmo visual y facilita el mantenimiento.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `rem` para `font-size` (escalable y accesible).
> - `clamp()` con una base en `rem` para tipografía fluida.
> - Define una escala tipográfica en variables y reúsala.
> - Recuerda que `font-size` fija la referencia de `em` y del `line-height` del elemento.

## Errores comunes

> [!warning] Trampas
> - **`px` en `font-size`**: ignora la preferencia del usuario.
> - **`em` anidado**: tamaños que se multiplican inesperadamente.
> - **Tamaños arbitrarios** sin una escala coherente.

## Notas relacionadas

- [[02 Relativas a Fuente (em, rem, ch, ex)]] — las unidades clave.
- [[07 line-height]] — el interlineado, que depende de `font-size`.
- [[02 clamp() para Tipografía Fluida]] — tipografía responsiva.
