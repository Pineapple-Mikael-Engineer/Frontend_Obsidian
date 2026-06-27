---
title: position static — El valor por defecto
aliases:
  - position static
tags:
  - css
  - api/propiedad
  - layout
propiedad: position
grupo: layout
valor_inicial: static
hereda: false
animable: false
draft: false
order: 1
---

# position static

> [!definicion]
> `position: static` es el valor **por defecto** de todos los elementos: el elemento se coloca en su sitio normal dentro del [[01 Flujo Normal del Documento/index | flujo del documento]], sin posicionamiento especial. Es "no posicionado".

```css
.normal { position: static; }   /* = el comportamiento por defecto */
```

## Características

| Aspecto | Comportamiento |
|---------|----------------|
| Posición | La que le da el flujo normal |
| `top`/`right`/`bottom`/`left` | **Se ignoran** |
| `z-index` | Se ignora |
| Sale del flujo | No |

## top/left no funcionan en static

> [!warning] Las propiedades de desplazamiento se ignoran
> Un elemento `static` **ignora** `top`, `right`, `bottom`, `left` y `z-index`. Estas propiedades solo tienen efecto en elementos **posicionados** (`relative`, `absolute`, `fixed`, `sticky`). Si pones `top: 20px` y no se mueve, casi siempre es porque el elemento sigue en `static`:
> ```css
> .x { top: 20px; }                      /* ❌ no hace nada (static) */
> .x { position: relative; top: 20px; }  /* ✅ ahora sí se desplaza */
> ```

## Para qué sirve declararlo

Como es el valor por defecto, rara vez se escribe `position: static` explícitamente. Su uso real es **anular** un posicionamiento heredado o aplicado antes, devolviendo el elemento al flujo normal:

```css
.elemento { position: absolute; }       /* en un estado */
.elemento.reset { position: static; }   /* volver al flujo normal */
```

## No crea contexto para hijos absolute

> [!info] static no sirve de referencia
> Un detalle clave: un elemento `static` **no** sirve de referencia para un hijo [[03 absolute | `absolute`]]. El hijo absolute busca el ancestro **posicionado** más cercano (relative/absolute/fixed/sticky), saltándose los static. Por eso, para que un contenedor "ancle" a sus hijos absolutos, debe ser `relative` (u otro no-static), no `static`.

## Buenas prácticas

> [!tip] Recomendaciones
> - No necesitas declarar `static`: es el valor por defecto.
> - Úsalo para **anular** un posicionamiento aplicado antes.
> - Recuerda que `top`/`left`/`z-index` no funcionan en `static`.
> - Para que un contenedor ancle hijos absolutos, usa `relative`, no `static`.

## Errores comunes

> [!warning] Trampas
> - **`top`/`left` sin efecto**: el elemento sigue en `static`.
> - **Esperar que un padre `static` ancle** un hijo `absolute`: no lo hace.

## Notas relacionadas

- [[02 relative]] — el primer valor "posicionado".
- [[06 Desplazamiento (top, right, bottom, left)]] — las propiedades que `static` ignora.
- [[01 Flujo Normal del Documento/index]] — el flujo donde `static` coloca el elemento.
