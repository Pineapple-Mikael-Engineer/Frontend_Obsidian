---
title: display none — Quitar un elemento del flujo
aliases:
  - display none
tags:
  - css
  - api/propiedad
  - layout
propiedad: display
grupo: layout
valor_inicial: inline
hereda: false
animable: false
draft: false
order: 4
---

# display none

> [!definicion]
> `display: none` **elimina** un elemento por completo: no se renderiza, no ocupa espacio y desaparece del flujo, como si no existiera en el DOM. También lo quita del **árbol de accesibilidad** (los lectores de pantalla lo ignoran).

```css
.oculto { display: none; }
.menu-movil { display: none; }   /* oculto en escritorio, visible en móvil con media query */
```

## none vs. visibility: hidden vs. opacity: 0

> [!info] Tres formas de "ocultar", muy distintas
> | Técnica | ¿Ocupa espacio? | ¿Lectores? | ¿Eventos? |
> |---------|-----------------|------------|-----------|
> | `display: none` | **No** | No | No |
> | `visibility: hidden` | **Sí** (deja el hueco) | No | No |
> | `opacity: 0` | Sí | **Sí** | **Sí** (sigue clicable) |
> | clase `sr-only` | No | **Sí** | Sí |
>
> - `display: none` desaparece del todo (ni hueco ni accesibilidad).
> - `visibility: hidden` se oculta pero **deja su hueco** reservado.
> - `opacity: 0` es invisible pero **sigue ahí** (ocupa espacio, recibe clics, lo leen los lectores) — útil para transiciones.

## Mostrar/ocultar con responsive

El uso más común: ocultar elementos según el tamaño de pantalla, alternando `display`:

```css
.menu-escritorio { display: flex; }
.menu-movil { display: none; }

@media (max-width: 768px) {
  .menu-escritorio { display: none; }
  .menu-movil { display: block; }
}
```

## No es animable

> [!warning] display no se puede transicionar (clásicamente)
> `display` cambia de forma **discreta** (none ↔ block), así que **no se puede animar** con `transition` de la forma clásica: el elemento aparece/desaparece de golpe. Para animar la entrada/salida, se usaban trucos (animar `opacity`/`height` y cambiar `display` al final). Las versiones modernas de CSS (`transition-behavior: allow-discrete` y `@starting-style`) permiten transicionar `display`, pero el soporte aún crece. Para un fade clásico, anima `opacity` y `visibility`, no `display`.

## Accesibilidad

> [!info] none oculta también a la asistencia
> `display: none` lo retira del árbol de accesibilidad: los lectores de pantalla **no lo leen**. Esto es correcto para contenido que no debe percibirse en ese momento (un menú cerrado). Pero si quieres ocultar **solo a la vista** manteniéndolo para lectores (un texto de contexto), usa la clase [[03 Contenido Oculto Visualmente (sr-only) | `sr-only`]], no `display: none`.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `display: none` para ocultar por completo (responsive, paneles cerrados).
> - Para mantener el hueco, usa `visibility: hidden`.
> - Para ocultar a la vista pero no a lectores, usa `sr-only`.
> - Para animar entrada/salida, anima `opacity`/`visibility`, no `display`.

## Errores comunes

> [!warning] Trampas
> - **Animar `display`** con transition clásica: no funciona (cambio discreto).
> - **Usar `display: none`** para algo que debe leer la asistencia: usa `sr-only`.
> - **Confundirlo con `visibility: hidden`**: este deja el hueco.

## Notas relacionadas

- [[04 focus-visible]] — el foco, que `display: none` también elimina.
- [[03 Contenido Oculto Visualmente (sr-only)]] — ocultar a la vista pero no a lectores.
- [[05 Ocultar (hidden)]] — el atributo HTML `hidden`, equivalente a `display: none`.
