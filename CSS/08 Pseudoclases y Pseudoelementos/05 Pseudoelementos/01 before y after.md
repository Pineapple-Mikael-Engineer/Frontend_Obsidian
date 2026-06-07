---
title: before y after — Contenido generado
aliases:
  - "::before"
  - "::after"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: "::before"
grupo: pseudoelemento
draft: false
---

# ::before y ::after

> [!definicion]
> `::before` y `::after` insertan **contenido generado** justo antes o después del contenido de un elemento, sin tocar el HTML. Requieren la propiedad [[02 Propiedad content | `content`]] (aunque sea vacía) para aparecer. Son los pseudoelementos más usados: iconos, comillas, etiquetas, formas decorativas.

```css
.externo::after { content: " ↗"; }
.cita::before { content: "“"; }
```

## Requieren content

> [!warning] Sin content, no existen
> `::before` y `::after` **no se renderizan** si no tienen la propiedad `content`. Es su requisito absoluto, incluso para formas puramente decorativas (donde se usa `content: ""`):
> ```css
> .decoracion::before {
>   content: "";            /* obligatorio, aunque esté vacío */
>   display: block;
>   width: 40px; height: 4px; background: #cba6f7;
> }
> ```
> Olvidar `content` es el error nº 1: el pseudoelemento simplemente no aparece. Detalle en [[02 Propiedad content]].

## Son hijos del elemento

> [!info] ::before es el primer hijo, ::after el último
> Conceptualmente, `::before` se inserta como el **primer hijo** del elemento y `::after` como el **último**, dentro de él:
> ```
> <div>::before  [contenido real]  ::after</div>
> ```
> Por eso heredan estilos del elemento y participan en su layout (se pueden posicionar, hacer flex items, etc.).

## Usos comunes

```css
/* Icono tras enlaces externos */
a[target="_blank"]::after { content: " ↗"; font-size: 0.8em; }

/* Comillas tipográficas en citas */
blockquote::before { content: open-quote; }
blockquote::after  { content: close-quote; }

/* Etiqueta "Nuevo" decorativa */
.nuevo::before { content: "NUEVO"; background: #cba6f7; padding: 0.1rem 0.4rem; border-radius: 4px; }

/* Forma decorativa (sin texto) */
.titulo::after { content: ""; display: block; width: 3rem; height: 3px; background: currentColor; }

/* Clearfix clásico */
.clearfix::after { content: ""; display: block; clear: both; }
```

## El contenido generado no es accesible (a veces)

> [!warning] No pongas información esencial en ::before/::after
> El contenido de `::before`/`::after` es **decorativo** desde el punto de vista de la accesibilidad: algunos lectores de pantalla lo leen y otros no, de forma inconsistente. **Nunca** pongas información **esencial** (texto que el usuario necesite) solo en un pseudoelemento. Úsalos para decoración (iconos, formas, comillas); el contenido importante va en el HTML real:
> ```css
> /* ✅ decorativo */
> .precio::before { content: "€"; }
> /* ❌ información esencial solo en CSS */
> .estado::after { content: "Pedido enviado"; }   /* debería estar en el HTML */
> ```

## Posicionar y animar

`::before`/`::after` se posicionan y animan como cualquier elemento, lo que permite efectos decorativos complejos (subrayados animados, badges, tooltips) sin marcado extra:

```css
.link { position: relative; }
.link::after {
  content: ""; position: absolute; bottom: -2px; left: 0;
  width: 100%; height: 2px; background: currentColor;
  transform: scaleX(0); transform-origin: left;
  transition: transform 0.2s;
}
.link:hover::after { transform: scaleX(1); }   /* subrayado que crece al pasar el ratón */
```

## Buenas prácticas

> [!tip] Recomendaciones
> - **Siempre** incluye `content` (aunque sea `""`) o el pseudoelemento no aparece.
> - Úsalos para **decoración** (iconos, formas, comillas, subrayados), no para contenido esencial.
> - Posiciónalos con `position: absolute` sobre un padre `relative` para efectos.
> - Para iconos accesibles que aportan significado, usa HTML real o `aria`.

## Errores comunes

> [!warning] Trampas
> - **Olvidar `content`**: el pseudoelemento no se renderiza.
> - **Información esencial** solo en `::before`/`::after`: inaccesible.
> - **Encadenar** `::before::after`: no se puede (uno por selector).

## Notas relacionadas

- [[02 Propiedad content]] — qué generar (texto, attr(), comillas, vacío).
- [[03 Clearfix]] — el `::after` del clearfix clásico.
- [[index]] — los pseudoelementos en general.
