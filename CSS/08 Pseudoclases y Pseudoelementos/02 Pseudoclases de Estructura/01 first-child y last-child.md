---
title: first-child y last-child — El primero y el último hijo
aliases:
  - ":first-child"
  - ":last-child"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":first-child"
grupo: pseudoclase
draft: false
order: 1
---

# :first-child y :last-child

> [!definicion]
> `:first-child` selecciona un elemento si es el **primer hijo** de su padre; `:last-child`, si es el **último**. Son las pseudoclases estructurales más usadas, sobre todo para ajustar los bordes de listas y los márgenes de los extremos.

```css
li:first-child { border-top: none; }
li:last-child  { border-bottom: none; }
```

## Usos típicos

```css
/* Quitar el borde superior del primer elemento y el inferior del último */
.lista li { border-bottom: 1px solid #eee; }
.lista li:last-child { border-bottom: none; }

/* Quitar el margen superior del primer párrafo de un bloque */
.contenido > :first-child { margin-top: 0; }

/* Redondear solo las esquinas de los extremos en un grupo de botones */
.grupo button:first-child { border-radius: 8px 0 0 8px; }
.grupo button:last-child  { border-radius: 0 8px 8px 0; }
```

## El detalle: cuenta TODOS los hermanos

> [!warning] :first-child mira la posición, no el tipo
> `:first-child` exige que el elemento sea **literalmente el primer hijo**, sin importar su tipo. Si antes hay otro elemento de distinto tipo, **no coincide**:
> ```html
> <div>
>   <h2>Título</h2>   <!-- este es el primer hijo -->
>   <p>Texto</p>      <!-- NO es :first-child -->
> </div>
> ```
> ```css
> p:first-child { }   /* ❌ no selecciona el <p> (el primer hijo es el <h2>) */
> p:first-of-type { } /* ✅ selecciona el primer <p> */
> ```
> Para "el primero de su tipo", usa [[02 first-of-type y last-of-type | `:first-of-type`]].

## first-child y gap hacen los márgenes innecesarios

> [!tip] Con gap, menos :first/:last-child
> Antes, separar elementos requería márgenes y quitar el sobrante del primero/último con `:first-child`/`:last-child`. Con [[07 Espaciado (gap) | `gap`]] (en Flexbox/Grid), el espacio va solo **entre** elementos, sin sobrantes en los extremos, así que muchos de estos ajustes ya no hacen falta:
> ```css
> /* Antiguo */
> li { margin-bottom: 1rem; }
> li:last-child { margin-bottom: 0; }
> /* Moderno */
> ul { display: flex; flex-direction: column; gap: 1rem; }
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `:first-child`/`:last-child` para los bordes y esquinas de los extremos de listas/grupos.
> - Recuerda que cuentan **todos** los hermanos: si importa el tipo, usa `:first-of-type`.
> - Con `gap`, muchos ajustes de margen en los extremos ya no son necesarios.
> - `:first-child` sobre el contenido para quitar el margen superior del primer bloque.

## Errores comunes

> [!warning] Trampas
> - **`p:first-child` que no coincide** porque el primer hijo es de otro tipo.
> - **Usarlo para márgenes** que `gap` resolvería mejor.

## Notas relacionadas

- [[02 first-of-type y last-of-type]] — el primero/último **de su tipo**.
- [[03 nth-child()]] — selección por posición con fórmulas.
- [[07 Espaciado (gap)]] — el espaciado que reduce su necesidad.
