---
title: position absolute — Anclar a un ancestro, fuera del flujo
aliases:
  - position absolute
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
order: 3
---

# position absolute

> [!definicion]
> `position: absolute` saca el elemento **del flujo normal** (deja de ocupar espacio) y lo posiciona con `top`/`left`… respecto al **ancestro posicionado más cercano** (el primero que no sea `static`). Si no hay ninguno, se ancla al bloque inicial (cerca del viewport). Es la base de superposiciones precisas.

```css
.contenedor { position: relative; }
.badge {
  position: absolute;
  top: 0.5rem; right: 0.5rem;
}
```

## El ancestro de referencia

> [!info] Busca el primer ancestro posicionado
> Un elemento `absolute` no se ancla a su padre directo, sino al **ancestro posicionado más cercano**: el primer antecesor con `position` distinto de `static` (relative, absolute, fixed o sticky). Subiendo por el árbol:
> - Si encuentra uno → se ancla a él.
> - Si no encuentra ninguno → se ancla al **bloque contenedor inicial** (≈ el `<html>`, cerca del viewport).
>
> Por eso el patrón es poner `position: relative` en el contenedor que quieres como referencia. Olvidarlo hace que el elemento "salte" a una posición inesperada.

## Sale del flujo

> [!warning] absolute no ocupa espacio
> Al posicionar en `absolute`, el elemento **se retira del flujo**: los demás elementos se comportan como si no existiera (se cierra su hueco). Esto permite superponerlo a otros, pero también significa que **no empuja** ni reserva espacio. Dos elementos `absolute` pueden solaparse sin saberlo.

## Centrar un absolute

> [!tip] Recetas de centrado con absolute
> Centrar un elemento absoluto en su contenedor tiene dos recetas clásicas:
> ```css
> /* Con transform (tamaño desconocido) */
> .centro {
>   position: absolute;
>   top: 50%; left: 50%;
>   transform: translate(-50%, -50%);
> }
>
> /* Con inset: 0 + margin auto (tamaño conocido) */
> .centro2 {
>   position: absolute;
>   inset: 0;
>   margin: auto;
>   width: 200px; height: 100px;
> }
> ```

## Casos de uso

```css
/* Badge de notificación sobre un icono */
.icono { position: relative; }
.icono .punto {
  position: absolute; top: -4px; right: -4px;
  width: 10px; height: 10px; border-radius: 50%; background: red;
}

/* Overlay que cubre toda una tarjeta */
.tarjeta { position: relative; }
.tarjeta .overlay { position: absolute; inset: 0; }

/* Etiqueta "Nuevo" en una esquina */
.producto .nuevo { position: absolute; top: 0; left: 0; }
```

## inset: el atajo moderno

[[06 Desplazamiento (top, right, bottom, left) | `inset`]] abrevia los cuatro lados: `inset: 0` equivale a `top: 0; right: 0; bottom: 0; left: 0`, estirando el elemento a todo el contenedor de referencia (ideal para overlays).

## Buenas prácticas

> [!tip] Recomendaciones
> - Pon `position: relative` en el contenedor de referencia (sin él, el absolute "salta").
> - Úsalo para superposiciones puntuales (badges, overlays, tooltips), no para el layout general.
> - `inset: 0` para cubrir todo el contenedor de referencia.
> - Para centrar, `top/left: 50%` + `transform: translate(-50%, -50%)`.

## Errores comunes

> [!warning] Trampas
> - **Olvidar el `relative` en el padre**: el absolute se ancla al viewport o a un ancestro lejano.
> - **Maquetar el layout con `absolute`**: frágil, no responsivo.
> - **Solapamientos inesperados**: como no ocupa espacio, dos absolutes pueden pisarse.

## Notas relacionadas

- [[02 relative]] — el contenedor de referencia habitual.
- [[04 fixed]] — anclado a la ventana en vez de a un ancestro.
- [[06 Desplazamiento (top, right, bottom, left)]] — `inset` y los offsets.
