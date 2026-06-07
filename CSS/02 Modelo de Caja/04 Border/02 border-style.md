---
title: border-style — Estilo del borde
aliases:
  - border-style
tags:
  - css
  - api/propiedad
  - layout
propiedad: border-style
grupo: box-model
valor_inicial: none
hereda: false
animable: false
draft: false
---

# border-style

> [!definicion]
> `border-style` define **cómo se dibuja** la línea del borde: sólida, punteada, discontinua, doble, etc. Es la propiedad **imprescindible**: vale `none` por defecto, así que sin ella el borde no se muestra aunque haya grosor y color.

```css
.caja { border: 2px solid; }       /* el más común */
.aviso { border: 2px dashed crimson; }
```

## Valores

| Valor | Aspecto |
|-------|---------|
| `none` | Sin borde (por defecto) |
| `solid` | Línea continua (el más usado) |
| `dashed` | Línea de guiones |
| `dotted` | Línea de puntos |
| `double` | Dos líneas paralelas |
| `groove` | Efecto "tallado" hacia dentro (3D) |
| `ridge` | Efecto "relieve" hacia fuera (3D) |
| `inset` | Hundido |
| `outset` | Resaltado |
| `hidden` | Como `none`, pero con prioridad en tablas |

> [!info] Los estilos 3D están en desuso
> `groove`, `ridge`, `inset` y `outset` crean efectos de bisel tridimensional que recuerdan al diseño de los años 90/2000. Hoy se usan poco: el diseño moderno prefiere [[03 Sombras (box-shadow) | sombras]] y bordes planos. Los útiles en la práctica son `solid`, `dashed`, `dotted` y `double`.

## El error clásico: olvidar el estilo

> [!warning] Sin border-style no hay borde visible
> Como `border-style` es `none` por defecto, definir solo grosor y color **no muestra nada**:
> ```css
> .x { border-width: 3px; border-color: blue; }  /* ❌ invisible */
> .x { border: 3px solid blue; }                 /* ✅ */
> ```
> Por eso casi siempre se usa el [[04 Shorthand border | shorthand `border`]], que obliga a pensar en los tres valores juntos.

## Estilo por lado

Cada lado puede tener su estilo, lo que permite efectos como pestañas o subrayados de borde:

```css
.tab-activa {
  border: 1px solid #ccc;
  border-bottom-style: none;   /* abre la pestaña por abajo */
}
```

## double y su reparto

`double` dibuja **dos** líneas con un hueco entre ellas, y el `border-width` se reparte entre las dos líneas y el espacio (a partes iguales). Necesita al menos 3px de grosor para verse bien:

```css
.elegante { border: 4px double #cba6f7; }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Declara siempre `border-style` (o usa el shorthand `border`) o el borde no se verá.
> - En la práctica: `solid`, `dashed`, `dotted`, `double`. Evita los estilos 3D.
> - Para resaltar al enfocar, recuerda que [[01 outline | `outline`]] no ocupa espacio (mejor que cambiar el borde).
> - Estilo por lado para pestañas y subrayados de borde.

## Errores comunes

> [!warning] Trampas
> - **Olvidar el estilo**: el borde no aparece.
> - **`double` con poco grosor**: las dos líneas no se distinguen.
> - **Estilos 3D** que dan un aspecto anticuado.

## Notas relacionadas

- [[01 border-width]] — el grosor.
- [[04 Shorthand border]] — declarar los tres valores juntos.
- [[01 outline]] — el contorno, que no desplaza el layout.
