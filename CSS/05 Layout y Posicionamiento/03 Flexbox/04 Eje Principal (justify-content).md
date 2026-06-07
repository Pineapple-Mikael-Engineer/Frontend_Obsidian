---
title: justify-content — Distribuir en el eje principal
aliases:
  - justify-content
tags:
  - css
  - api/propiedad
  - layout
propiedad: justify-content
grupo: flexbox
valor_inicial: flex-start
hereda: false
animable: false
draft: false
---

# justify-content

> [!definicion]
> `justify-content` distribuye los items a lo largo del **eje principal** del contenedor flex (horizontal en `row`, vertical en `column`): los agrupa al inicio, al final, al centro, o reparte el espacio sobrante entre ellos. Es una propiedad del **contenedor**.

```css
.barra { display: flex; justify-content: space-between; }
```

## Valores

| Valor | Distribución |
|-------|--------------|
| `flex-start` | Agrupados al inicio (por defecto) |
| `flex-end` | Agrupados al final |
| `center` | Centrados |
| `space-between` | Repartidos: primero y último pegados a los bordes, huecos iguales entre medias |
| `space-around` | Huecos iguales alrededor de cada item (medio hueco en los extremos) |
| `space-evenly` | Huecos **perfectamente** iguales, incluidos los extremos |

## space-between, space-around, space-evenly

> [!info] Las tres distribuciones con espacio
> La diferencia entre las tres "space-*":
> ```
> space-between:  [A]____[B]____[C]      (sin hueco en los bordes)
> space-around:   _[A]__[B]__[C]_        (medio hueco en los bordes)
> space-evenly:   __[A]__[B]__[C]__      (hueco igual en todo, incluidos bordes)
> ```
> - `space-between`: primero y último **pegados a los bordes**. Ideal para una navbar (logo izquierda, menú derecha).
> - `space-evenly`: huecos idénticos en todas partes. El más "equilibrado".
> - `space-around`: intermedio (los bordes tienen la mitad de hueco).

## Recetas comunes

```css
/* Navbar: logo a un lado, acciones al otro */
.navbar { display: flex; justify-content: space-between; align-items: center; }

/* Botones centrados */
.acciones { display: flex; justify-content: center; gap: 1rem; }

/* Distribución uniforme de elementos */
.menu { display: flex; justify-content: space-evenly; }

/* Empujar un item al final (alternativa: margin-left auto) */
.barra { display: flex; justify-content: flex-end; }
```

## El eje depende de flex-direction

> [!warning] justify-content actúa en el eje PRINCIPAL
> `justify-content` siempre opera en el eje **principal**, que cambia con [[02 Dirección (flex-direction) | `flex-direction`]]:
> - En `row` → distribuye **horizontalmente**.
> - En `column` → distribuye **verticalmente**.
>
> Por eso, para centrar verticalmente en una columna, usas `justify-content: center`; en una fila, `align-items: center`. Es el origen de mucha confusión.

## justify-content vs. margin: auto

Para empujar **un** item al extremo (no distribuir todos), `margin-left: auto` (en row) suele ser más limpio que cambiar `justify-content`:

```css
.navbar .perfil { margin-left: auto; }   /* solo este item va a la derecha */
```

## Buenas prácticas

> [!tip] Recomendaciones
> - `space-between` para "extremos opuestos" (navbar logo/menú).
> - `center` para agrupar y centrar; `space-evenly` para distribución uniforme.
> - Recuerda que actúa en el eje **principal** (cambia con `flex-direction`).
> - Para empujar un solo item, `margin: auto` en ese item.

## Errores comunes

> [!warning] Trampas
> - **Confundir el eje**: en `column`, `justify-content` es vertical.
> - **Usar `justify-content`** para mover un solo item (mejor `margin: auto`).
> - **Esperar `space-evenly`** y usar `space-around` (los bordes salen distintos).

## Notas relacionadas

- [[05 Eje Transversal (align-items, align-self)]] — el eje perpendicular.
- [[02 Dirección (flex-direction)]] — define cuál es el eje principal.
- [[07 Espaciado (gap)]] — espacio fijo entre items, combinable con justify.
