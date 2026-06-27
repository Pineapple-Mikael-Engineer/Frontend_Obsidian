---
title: Contenedor Flex (display flex) — Activar Flexbox
aliases:
  - display flex
  - flex container
tags:
  - css
  - api/propiedad
  - layout
propiedad: display
grupo: flexbox
valor_inicial: inline
hereda: false
animable: false
draft: false
order: 1
---

# Contenedor Flex (display flex)

> [!definicion]
> `display: flex` convierte un elemento en **contenedor flex**: sus **hijos directos** se vuelven **items flex** y se disponen en una fila (por defecto), repartiendo el espacio según las propiedades flex. Es el interruptor que activa todo el sistema Flexbox.

```css
.barra {
  display: flex;
  gap: 1rem;
}
```

## Qué cambia al declararlo

> [!info] El contenedor y sus hijos cambian de comportamiento
> Poner `display: flex` tiene efectos inmediatos:
> - Los **hijos directos** se colocan **en una fila** (por defecto), uno al lado de otro, sin importar si eran `block` o `inline`.
> - Los hijos pueden **alinearse** y **repartir el espacio** con las propiedades flex.
> - El `display` original de los hijos (block/inline) **deja de importar**: todos se vuelven items flex.
> - Los márgenes verticales de los hijos **dejan de colapsar**.

## flex vs. inline-flex

| Valor | El contenedor es… |
|-------|-------------------|
| `flex` | Un bloque (ocupa todo el ancho disponible) |
| `inline-flex` | En línea (ocupa solo su contenido, fluye con el texto) |

`inline-flex` es útil para un grupo flex que debe convivir con texto en la misma línea; `flex` para la mayoría de contenedores.

## Solo afecta a los hijos directos

> [!warning] Flexbox no es recursivo
> `display: flex` solo convierte en items a los **hijos directos** del contenedor, no a los nietos. Para que un item flex organice **su** contenido con flexbox, hay que declararle también `display: flex`. Es común anidar contenedores flex (una barra flex que contiene grupos, cada uno flex).

## El contenedor mínimo

```css
.flex {
  display: flex;
  gap: 1rem;          /* espacio entre items */
  align-items: center; /* alinear verticalmente */
}
```

Estas tres líneas cubren la mayoría de los casos: items en fila, separados, centrados verticalmente.

## Ejemplos de uso

```css
/* Barra de navegación: logo a un lado, menú al otro */
.navbar { display: flex; justify-content: space-between; align-items: center; }

/* Grupo de botones */
.acciones { display: flex; gap: 0.5rem; }

/* Centrar un único elemento */
.centro { display: flex; justify-content: center; align-items: center; }

/* Icono + texto alineados */
.con-icono { display: flex; align-items: center; gap: 0.5rem; }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - `display: flex` + `gap` + `align-items` cubre la mayoría de los componentes lineales.
> - Usa `inline-flex` solo si el contenedor debe fluir con el texto.
> - Para organizar nietos, declara flex también en los hijos (anidamiento).
> - Para layouts bidimensionales (filas **y** columnas), usa Grid, no flex anidado.

## Errores comunes

> [!warning] Trampas
> - **Esperar que afecte a los nietos**: solo a los hijos directos.
> - **Olvidar que el `display` de los hijos** ya no importa (todos son items flex).
> - **Usar flex para rejillas 2D**: Grid es mejor.

## Notas relacionadas

- [[02 Dirección (flex-direction)]] — fila o columna.
- [[07 Espaciado (gap)]] — el espacio entre items.
- [[10 Casos de Uso]] — recetas completas.
