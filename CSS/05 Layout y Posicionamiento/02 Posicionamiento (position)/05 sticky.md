---
title: position sticky — Pegajoso al hacer scroll
aliases:
  - position sticky
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
---

# position sticky

> [!definicion]
> `position: sticky` es un **híbrido**: el elemento se comporta como `relative` (en el flujo normal) **hasta** que el scroll alcanza un umbral (`top`, `bottom`…), y entonces se "pega" como `fixed` mientras su contenedor esté visible. Es la base de cabeceras pegajosas, índices laterales y encabezados de tabla fijos.

```css
.encabezado {
  position: sticky;
  top: 0;   /* se pega al llegar a 0px del borde superior */
}
```

## El umbral es obligatorio

> [!warning] sticky sin top/bottom no hace nada
> `position: sticky` **necesita** al menos un umbral (`top`, `bottom`, `left` o `right`) que indique **dónde** se pega. Sin él, el elemento se comporta como `relative` y nunca se "pega":
> ```css
> .x { position: sticky; }          /* ❌ no se pega (falta el umbral) */
> .x { position: sticky; top: 0; }  /* ✅ se pega al borde superior */
> ```

## Cómo funciona

> [!info] Relative, luego fixed, dentro de su contenedor
> El comportamiento en tres fases:
> 1. **Normal**: mientras el elemento está por debajo del umbral, fluye con la página (como `relative`).
> 2. **Pegado**: cuando el scroll lo lleva al umbral (`top: 0`), se "pega" ahí y deja de moverse (como `fixed`).
> 3. **Liberado**: cuando su **contenedor padre** sale de la vista, el elemento se va con él (no se queda pegado para siempre).
>
> A diferencia de `fixed`, sticky **sí reserva su espacio** y está **acotado a su contenedor**.

## sticky vs. fixed

| | `sticky` | `fixed` |
|--|----------|---------|
| Reserva espacio | **Sí** | No |
| Acotado a su contenedor | **Sí** | No (toda la pantalla) |
| Antes del umbral | Fluye normal | Siempre fijo |
| Umbral obligatorio | Sí | No |

Para una cabecera de sitio, ambas valen; `sticky` suele ser más limpio porque reserva su espacio y no tapa contenido.

## Casos de uso

```css
/* Cabecera de sitio pegajosa */
.navbar { position: sticky; top: 0; z-index: 10; }

/* Encabezado de tabla fijo al hacer scroll */
thead th { position: sticky; top: 0; background: #181825; }

/* Índice lateral que sigue mientras dura su sección */
.toc { position: sticky; top: 2rem; }

/* Subtítulos de sección "pegados" mientras dura cada grupo */
.grupo h2 { position: sticky; top: 0; }
```

## Por qué a veces "no funciona"

> [!warning] Las causas frecuentes del sticky que falla
> Si un `sticky` no se pega, suele ser por:
> - **Falta el umbral** (`top`/`bottom`).
> - Un **ancestro con `overflow: hidden`/`auto`/`scroll`**: crea un contexto de scroll que confina el sticky; el elemento se pega respecto a ese ancestro, no a la ventana.
> - El **contenedor no es más alto** que el elemento: no hay margen para pegarse.
> - Una **altura fija** en un ancestro que recorta el área de pegado.

## Buenas prácticas

> [!tip] Recomendaciones
> - Define **siempre** un umbral (`top: 0`).
> - Dale un fondo (las celdas/cabeceras sticky necesitan `background` para no transparentar el contenido que pasa por detrás).
> - Añade `z-index` para que quede por encima del contenido al pegarse.
> - Si no funciona, revisa `overflow` en los ancestros.
> - Respeta `scroll-margin`/`scroll-padding` para anclas bajo cabeceras sticky.

## Errores comunes

> [!warning] Trampas
> - **Sin umbral**: no se pega.
> - **Ancestro con `overflow`** que confina o rompe el sticky.
> - **Sin fondo**: el contenido se ve a través de la cabecera pegada.

## Notas relacionadas

- [[04 fixed]] — fijo siempre, sin reservar espacio.
- [[02 relative]] — el comportamiento de sticky antes del umbral.
- [[07 Secciones (thead, tbody, tfoot)]] — encabezados de tabla pegajosos.
