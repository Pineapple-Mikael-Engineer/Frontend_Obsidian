---
title: Selector, Propiedad y Valor — Las tres piezas mínimas
aliases:
  - selector property value
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# Selector, Propiedad y Valor

> [!definicion]
> Toda regla CSS se construye con tres piezas: el **selector** (a qué elementos afecta), la **propiedad** (qué característica se cambia) y el **valor** (qué valor toma esa característica). Entender estas tres es entender CSS.

```css
p { color: navy; }
/* └selector
       └propiedad
              └valor */
```

## Selector: a qué elementos

El **selector** decide **qué** elementos del HTML reciben los estilos. Puede apuntar por tipo de elemento, por clase, por id, por relación con otros, etc. Es un tema rico que se desarrolla en su sección:

```css
h1            { }   /* todos los <h1> */
.destacado    { }   /* elementos con class="destacado" */
#menu         { }   /* el elemento con id="menu" */
nav a         { }   /* enlaces dentro de un <nav> */
```

El catálogo completo está en [[04 Selectores/index | Selectores]].

## Propiedad: qué característica

La **propiedad** nombra el aspecto visual a modificar. CSS tiene cientos: `color`, `font-size`, `margin`, `background`, `display`… Cada una acepta un conjunto concreto de valores:

```css
p {
  color: ...;        /* un color */
  font-size: ...;    /* un tamaño */
  text-align: ...;   /* left | center | right | justify */
}
```

## Valor: con qué valor

El **valor** es lo que se asigna a la propiedad, y su formato depende de la propiedad: un color (`crimson`, `#f00`), una longitud (`16px`, `2rem`), una palabra clave (`center`, `bold`), una función (`calc(100% - 2rem)`).

| Tipo de valor | Ejemplos |
|---------------|----------|
| Color | `red`, `#cba6f7`, `rgb(0 0 0)` |
| Longitud | `16px`, `2rem`, `50%` |
| Palabra clave | `center`, `bold`, `none` (propia de cada propiedad) |
| Función | `calc()`, `clamp()`, `var()` |

Los colores se desarrollan en [[05 Colores/index]] y las longitudes en [[06 Unidades de Medida/index]].

## Cómo encajan

Una declaración es **propiedad + valor**; una regla es **selector + declaraciones**. La separación entre el "qué" (selector) y el "cómo" (propiedad/valor) es lo que permite aplicar el mismo estilo a muchos elementos o muchos estilos a un elemento.

```css
.boton {                    /* a qué */
  background: #cba6f7;      /* cómo: fondo */
  padding: 0.5rem 1rem;     /* cómo: espaciado */
  border-radius: 0.5rem;    /* cómo: esquinas */
}
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Elige el selector más **estable** y de **baja especificidad** posible (clases, no ids; ver [[04 Selectores/index | Selectores]]).
> - Conoce qué valores acepta cada propiedad antes de usarla (la tabla de valores de su nota).
> - Una propiedad por línea, terminada en `;`, para legibilidad.

## Notas relacionadas

- [[02 Declaración y Bloque]] — cómo se agrupan en bloques.
- [[04 Selectores/index]] — la variedad de selectores.
- [[06 Unidades de Medida/index]] — los valores de longitud.
