---
title: position fixed — Fijar a la ventana
aliases:
  - position fixed
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
order: 4
---

# position fixed

> [!definicion]
> `position: fixed` ancla el elemento a la **ventana del navegador** (viewport): permanece en el mismo sitio de la pantalla **aunque se haga scroll**. Sale del flujo, como `absolute`, pero su referencia es siempre la ventana, no un ancestro. Es la base de barras fijas, botones flotantes y modales.

```css
.boton-flotante {
  position: fixed;
  bottom: 1.5rem; right: 1.5rem;
}
```

## fixed vs. absolute

> [!info] La ventana vs. un ancestro
> Ambos salen del flujo y se posicionan con `top`/`left`, pero su **referencia** difiere:
> - **`absolute`**: respecto al ancestro posicionado más cercano. Se mueve con el scroll de la página.
> - **`fixed`**: respecto al **viewport**. **No** se mueve con el scroll: queda clavado en la pantalla.
>
> | | `absolute` | `fixed` |
> |--|-----------|---------|
> | Referencia | Ancestro posicionado | La ventana |
> | ¿Se mueve al hacer scroll? | Sí | **No** |

## Casos de uso

```css
/* Barra de navegación siempre visible arriba */
.navbar { position: fixed; top: 0; left: 0; right: 0; }

/* Botón "volver arriba" o de acción flotante */
.fab { position: fixed; bottom: 1.5rem; right: 1.5rem; }

/* Banner de cookies abajo */
.cookies { position: fixed; bottom: 0; inset-inline: 0; }

/* Modal centrado en la pantalla */
.modal { position: fixed; inset: 0; display: grid; place-items: center; }
```

## Compensar el espacio que ocupa

> [!warning] fixed tapa el contenido
> Como `fixed` sale del flujo, **no reserva espacio**: una barra de navegación fija se **superpone** al contenido, tapando la parte superior de la página. Hay que compensar con padding/margin en el contenido:
> ```css
> .navbar { position: fixed; top: 0; height: 60px; }
> body { padding-top: 60px; }   /* deja sitio para que la barra no tape el contenido */
> ```
> Para muchos casos, [[05 sticky | `position: sticky`]] es mejor porque **sí** reserva espacio.

## La trampa de filter/transform en ancestros

> [!warning] Un ancestro con transform/filter rompe fixed
> Un detalle que sorprende: si un **ancestro** tiene `transform`, `filter` o `perspective` (distintos de `none`), el `fixed` deja de anclarse a la ventana y se ancla a ese ancestro (que crea un contexto de contención). El elemento "fijo" entonces **se mueve con el scroll**. Si un `fixed` no se queda quieto, busca un `transform`/`filter` en sus ancestros.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `fixed` para barras, botones flotantes, banners y modales que deben quedar en pantalla.
> - **Compensa** el espacio que ocupa (padding en el contenido) para no tapar nada.
> - Considera `sticky` si quieres "fijo" pero que reserve espacio.
> - Si un `fixed` se mueve con el scroll, revisa `transform`/`filter` en los ancestros.

## Errores comunes

> [!warning] Trampas
> - **Tapar el contenido** sin compensar el espacio.
> - **`fixed` que no se queda quieto**: por un `transform`/`filter` en un ancestro.
> - **`fixed` en móvil con el teclado abierto**: puede comportarse de forma rara.

## Notas relacionadas

- [[03 absolute]] — anclado a un ancestro, se mueve con el scroll.
- [[05 sticky]] — fijo al hacer scroll pero reservando espacio.
- [[01 filter]] — el efecto que puede "romper" un `fixed` desde un ancestro.
