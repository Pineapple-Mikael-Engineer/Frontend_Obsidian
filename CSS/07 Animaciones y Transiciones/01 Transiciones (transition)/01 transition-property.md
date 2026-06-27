---
title: transition-property — Qué propiedades se animan
aliases:
  - transition-property
tags:
  - css
  - api/propiedad
  - animacion
propiedad: transition-property
grupo: animacion
valor_inicial: all
hereda: false
animable: false
draft: false
order: 1
---

# transition-property

> [!definicion]
> `transition-property` indica **qué propiedades** participan en la transición: una lista concreta (`background, transform`), todas (`all`) o ninguna (`none`). Es la primera parte del shorthand `transition`.

```css
.x { transition-property: background, transform; }
```

## Valores

| Valor | Efecto |
|-------|--------|
| `all` | Transiciona cualquier propiedad animable que cambie (por defecto) |
| `none` | Ninguna transición |
| `<lista>` | Solo las propiedades nombradas: `background, color, transform` |

## Listar propiedades vs. all

> [!tip] Lista las propiedades en producción
> Aunque `all` es cómodo, en producción conviene **listar** las propiedades concretas:
> ```css
> /* ✅ explícito: solo lo que quieres animar */
> transition-property: transform, opacity;
> /* ⚠️ cómodo pero arriesgado */
> transition-property: all;
> ```
> Razones para evitar `all`:
> - Puede animar propiedades **inesperadas** que cambian a la vez (un reflow).
> - Algunas son **caras** de animar (layout, paint); listar evita animar las que no quieres.
> - Más **previsible** y fácil de depurar.

## Solo anima propiedades animables

`transition-property` solo tiene efecto sobre [[05 Propiedades Animables | propiedades animables]] (las que tienen valores interpolables). Listar una no animable (como `display`) no la transiciona de la forma clásica. La lista completa de qué se puede animar está en su nota.

## Cada propiedad puede tener su tiempo

Cuando se listan varias propiedades, las demás partes del shorthand (`duration`, `timing-function`, `delay`) se pueden dar como **listas paralelas**, una por propiedad:

```css
transition-property: transform, opacity;
transition-duration: 0.3s, 0.5s;          /* transform 0.3s, opacity 0.5s */
transition-timing-function: ease-out, linear;
```

(O, más legible, con el shorthand: `transition: transform 0.3s ease-out, opacity 0.5s linear;`)

## Buenas prácticas

> [!tip] Recomendaciones
> - **Lista** las propiedades concretas en producción, no `all`.
> - Prioriza animar `transform` y `opacity` (las más eficientes).
> - Si usas listas paralelas para duración/curva, asegúrate de que el orden coincida.
> - Usa el shorthand `transition` para mantener cada propiedad con su tiempo juntos.

## Errores comunes

> [!warning] Trampas
> - **`transition: all`** que anima cambios inesperados o caros.
> - **Listar una propiedad no animable** esperando una transición suave.
> - **Listas paralelas desalineadas** (más duraciones que propiedades, o al revés).

## Notas relacionadas

- [[05 Propiedades Animables]] — qué propiedades se pueden transicionar.
- [[02 transition-duration]] — la duración de cada una.
- [[index]] — el shorthand `transition` completo.
