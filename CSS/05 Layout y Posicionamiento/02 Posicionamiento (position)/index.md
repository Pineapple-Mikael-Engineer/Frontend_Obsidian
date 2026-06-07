---
title: Posicionamiento (position) — Sacar elementos del flujo
aliases:
  - position
tags:
  - css
  - api/concepto
  - layout
draft: false
---

# Posicionamiento (position)

> [!definicion]
> La propiedad `position` controla cómo se **posiciona** un elemento respecto al flujo normal: en su sitio (`static`), desplazado desde él (`relative`), anclado a un ancestro (`absolute`), fijo a la ventana (`fixed`) o pegajoso al hacer scroll (`sticky`). Con [[06 Desplazamiento (top, right, bottom, left) | `top`/`left`…]] se ajusta la posición, y [[07 z-index y Contexto de Apilamiento | `z-index`]] controla el apilado.

```css
.modal { position: fixed; inset: 0; }
.badge { position: absolute; top: -8px; right: -8px; }
.nav   { position: sticky; top: 0; }
```

## Los cinco valores

| Valor | Respecto a | Sale del flujo |
|-------|------------|----------------|
| `static` | Su sitio normal | No (por defecto) |
| `relative` | Su propia posición original | No (deja su hueco) |
| `absolute` | El ancestro posicionado más cercano | Sí |
| `fixed` | La ventana (viewport) | Sí |
| `sticky` | Híbrido: normal hasta un umbral, luego fijo | Parcial |

Cada valor en su nota: [[01 static]], [[02 relative]], [[03 absolute]], [[04 fixed]] y [[05 sticky]].

## La pareja relative + absolute

> [!tip] El patrón fundamental del posicionamiento
> El uso más común combina dos valores: un contenedor `relative` (que no se mueve, pero sirve de **referencia**) y un hijo `absolute` (que se ancla a ese contenedor). Es la base de badges, tooltips, botones flotantes sobre una tarjeta:
> ```css
> .tarjeta { position: relative; }          /* referencia */
> .tarjeta .badge {
>   position: absolute; top: 0.5rem; right: 0.5rem;   /* anclado a la tarjeta */
> }
> ```
> Sin el `relative` en el padre, el `absolute` se anclaría al viewport o a un ancestro lejano. Detalle en [[03 absolute]].

## Mapa de la subsección

- [[01 static]] — el valor por defecto (sin posicionar).
- [[02 relative]] — desplazar sin salir del flujo, y crear referencia.
- [[03 absolute]] — anclar a un ancestro, fuera del flujo.
- [[04 fixed]] — fijar a la ventana.
- [[05 sticky]] — pegajoso al hacer scroll.
- [[06 Desplazamiento (top, right, bottom, left)]] — ajustar la posición e `inset`.
- [[07 z-index y Contexto de Apilamiento]] — el orden de apilado en profundidad.

## position no es para layout general

> [!warning] Usa Flexbox/Grid para el layout, position para casos puntuales
> El posicionamiento (sobre todo `absolute`) **no** es la herramienta para maquetar la estructura de una página: hacerlo da layouts frágiles que no se adaptan. Para la estructura, usa [[03 Flexbox/index | Flexbox]] y [[04 CSS Grid/index | Grid]]. `position` se reserva para elementos que deben **salir del flujo**: superposiciones, modales, tooltips, badges, barras pegajosas.

## Notas relacionadas

- [[03 absolute]] · [[02 relative]] — la pareja más usada.
- [[05 sticky]] — el efecto pegajoso, muy popular.
- [[07 z-index y Contexto de Apilamiento]] — controlar qué queda encima.
