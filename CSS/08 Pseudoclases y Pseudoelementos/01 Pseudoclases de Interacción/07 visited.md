---
title: visited — Enlaces ya visitados
aliases:
  - ":visited"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":visited"
grupo: pseudoclase
draft: false
---

# :visited

> [!definicion]
> `:visited` selecciona los **enlaces** que el usuario **ya ha visitado** (están en el historial del navegador). Permite distinguir visualmente los enlaces vistos de los nuevos, una ayuda de navegación clásica. Por **privacidad**, el navegador limita severamente qué estilos se pueden aplicar.

```css
a:visited { color: #9d7cd8; }   /* un tono más apagado para enlaces vistos */
```

## La utilidad: orientación

> [!tip] Distinguir lo visto de lo nuevo
> `:visited` ayuda al usuario a saber **qué enlaces ya ha seguido**, útil en listas largas, resultados de búsqueda o índices. La convención clásica es un color más apagado o distinto:
> ```css
> a:link    { color: #7c3aed; }   /* sin visitar: color vivo */
> a:visited { color: #9d7cd8; }   /* visitado: más apagado */
> ```

## Las restricciones de privacidad

> [!warning] :visited solo permite cambiar colores
> Por una razón de **privacidad** importante, los navegadores **limitan drásticamente** qué se puede estilar con `:visited`. Sin esa restricción, una web maliciosa podría **detectar** qué sitios has visitado (leyendo los estilos aplicados con JS), revelando tu historial de navegación. Por eso:
> - Solo se pueden cambiar **propiedades de color**: `color`, `background-color`, `border-color`, `column-rule-color`, `outline-color`.
> - **No** se pueden cambiar layout, fuentes, fondos con imagen, ni nada que altere el tamaño o la geometría.
> - `getComputedStyle` **miente** sobre los estilos de `:visited` (devuelve los del estado sin visitar) para que JS no pueda detectarlos.
>
> ```css
> a:visited {
>   color: #9d7cd8;            /* ✅ permitido */
>   font-weight: bold;         /* ❌ se ignora (no es color) */
>   background: url(check.png); /* ❌ se ignora */
> }
> ```

## La opacidad de los colores también se limita

> [!info] Ni siquiera la transparencia escapa
> Para cerrar el resquicio, los navegadores tampoco permiten que el **canal alfa** de los colores de `:visited` revele información: aunque puedas cambiar el color, el navegador lo renderiza de forma que no se pueda detectar por diferencias sutiles. La privacidad del historial está muy protegida.

## El orden LVHA

> [!warning] :visited va segundo
> En la secuencia [[index | LVHA]], `:visited` va el **segundo** (tras `:link`):
> ```css
> a:link { } a:visited { } a:hover { } a:active { }
> ```
> Si va después de `:hover`, el color de visitado pisaría el de hover.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `:visited` solo para cambiar **colores** (es lo único permitido).
> - Distingue los enlaces visitados con un tono apagado para orientar al usuario.
> - Respeta el orden LVHA (`:visited` segundo).
> - No esperes poder cambiar layout/fuentes/fondos: la privacidad lo impide.

## Errores comunes

> [!warning] Trampas
> - **Intentar cambiar `font-weight`/`background-image`** en `:visited`: se ignora.
> - **`:visited` tras `:hover`** en el orden: pisa el hover.
> - **Esperar leer el estado con JS**: el navegador lo oculta por privacidad.

## Notas relacionadas

- [[index]] — el orden LVHA de las pseudoclases de enlace.
- [[01 hover]] · [[02 active]] — los otros estados del enlace.
- [[05 Enlaces y Navegación/index]] — los enlaces que `:visited` distingue.
