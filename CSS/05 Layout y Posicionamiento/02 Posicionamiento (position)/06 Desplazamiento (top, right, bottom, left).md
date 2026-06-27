---
title: Desplazamiento — top, right, bottom, left e inset
aliases:
  - inset
  - top right bottom left
tags:
  - css
  - api/propiedad
  - layout
propiedad: inset
grupo: layout
valor_inicial: auto
hereda: false
animable: true
draft: false
order: 6
---

# Desplazamiento (top, right, bottom, left)

> [!definicion]
> Las propiedades `top`, `right`, `bottom` y `left` definen la **distancia** desde los bordes del contenedor de referencia para un elemento **posicionado** (no `static`). `inset` es el atajo que las agrupa. Junto con `position`, determinan dónde se coloca exactamente el elemento.

```css
.x { position: absolute; top: 1rem; right: 1rem; }
.y { position: absolute; inset: 0; }   /* los cuatro a 0 */
```

## Solo funcionan en elementos posicionados

> [!warning] Inútiles en static
> `top`/`right`/`bottom`/`left` (e `inset`) **solo tienen efecto** si el elemento tiene `position` distinto de `static` (relative, absolute, fixed, sticky). En un elemento `static` (el valor por defecto) se **ignoran**. Es el error nº 1: poner `top: 20px` y que no pase nada porque falta el `position`.

## Respecto a qué se miden

La referencia depende del `position`:

| `position` | `top: 0` significa… |
|------------|---------------------|
| `relative` | 0 desde su **posición original** |
| `absolute` | 0 desde el borde del **ancestro posicionado** |
| `fixed` | 0 desde el borde de la **ventana** |
| `sticky` | El **umbral** al que pegarse |

## inset: el atajo

`inset` agrupa los cuatro lados, como `padding`/`margin`:

| Forma | Equivale a |
|-------|-----------|
| `inset: 0` | `top: 0; right: 0; bottom: 0; left: 0` |
| `inset: 1rem 2rem` | vertical · horizontal |
| `inset: 1rem 2rem 3rem 4rem` | T R B L |

> [!tip] inset: 0 para overlays a pantalla completa
> `inset: 0` en un elemento `absolute`/`fixed` lo **estira** a todo el contenedor de referencia, la forma más limpia de hacer overlays y modales que lo cubren todo:
> ```css
> .overlay { position: absolute; inset: 0; background: rgb(0 0 0 / 0.5); }
> .modal-fondo { position: fixed; inset: 0; }
> ```

## Las propiedades lógicas

Existen versiones **lógicas** que se adaptan a la dirección del texto: `inset-block` (vertical), `inset-inline` (horizontal), `inset-block-start`, etc. Útiles para soportar RTL:

```css
.badge { position: absolute; inset-block-start: 0.5rem; inset-inline-end: 0.5rem; }
/* esquina superior derecha en LTR, superior izquierda en RTL */
```

## Opuestos juntos = estirar

> [!info] top + bottom (o left + right) estira el elemento
> Si fijas **lados opuestos** (`top` y `bottom`, o `left` y `right`) en un elemento `absolute`/`fixed` **sin** un `height`/`width` fijo, el elemento se **estira** entre ambos:
> ```css
> .columna-lateral {
>   position: absolute;
>   top: 0; bottom: 0;   /* ocupa todo el alto del contenedor */
>   right: 0; width: 250px;
> }
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Recuerda que solo funcionan con `position` ≠ `static`.
> - `inset: 0` para overlays y modales a pantalla completa.
> - Usa propiedades lógicas (`inset-inline`) para soportar RTL.
> - Fija lados opuestos para estirar un elemento entre dos bordes.

## Errores comunes

> [!warning] Trampas
> - **`top`/`left` sin `position`**: no hacen nada (falta posicionar).
> - **Confundir la referencia**: `top: 0` mide desde distinto sitio según el `position`.
> - **Olvidar que `inset` es un shorthand**: resetea los cuatro lados.

## Notas relacionadas

- [[01 static]] — donde estas propiedades se ignoran.
- [[03 absolute]] · [[04 fixed]] — los contextos donde más se usan.
- [[07 z-index y Contexto de Apilamiento]] — el otro control del posicionamiento.
