---
title: Flexbox — Layout flexible en una dimensión
aliases:
  - flexbox
  - flex
tags:
  - css
  - api/concepto
  - layout
draft: false
---

# Flexbox

> [!definicion]
> **Flexbox** (Flexible Box Layout) organiza elementos en **una dimensión**: una fila **o** una columna. Reparte el espacio disponible entre los items, los alinea en ambos ejes y permite que crezcan o se encojan. Es el sistema preferido para **componentes lineales**: barras de navegación, botoneras, tarjetas, listas de etiquetas.

```css
.contenedor {
  display: flex;
  gap: 1rem;
  align-items: center;
  justify-content: space-between;
}
```

## Los dos ejes

> [!info] Eje principal y eje transversal
> Flexbox gira en torno a dos ejes:
> - **Eje principal** (main axis): la dirección en que se colocan los items, definida por [[02 Dirección (flex-direction) | `flex-direction`]] (fila u columna). Se controla con [[04 Eje Principal (justify-content) | `justify-content`]].
> - **Eje transversal** (cross axis): perpendicular al principal. Se controla con [[05 Eje Transversal (align-items, align-self) | `align-items`]].
>
> Entender que `justify-content` actúa en el eje **principal** y `align-items` en el **transversal** es la clave de Flexbox. Si cambias `flex-direction` a `column`, los ejes **se intercambian**: `justify-content` pasa a ser vertical y `align-items` horizontal.

## Contenedor e items

| Propiedades del **contenedor** | Propiedades de los **items** |
|--------------------------------|------------------------------|
| `display: flex` | `flex-grow`, `flex-shrink`, `flex-basis` |
| `flex-direction`, `flex-wrap` | `align-self` |
| `justify-content`, `align-items`, `align-content` | `order` |
| `gap` | — |

## Mapa de la subsección

- [[01 Contenedor Flex (display flex)]] — activar Flexbox.
- [[02 Dirección (flex-direction)]] — fila o columna.
- [[03 Ajuste de Línea (flex-wrap)]] — permitir varias líneas.
- [[04 Eje Principal (justify-content)]] — distribuir en el eje principal.
- [[05 Eje Transversal (align-items, align-self)]] — alinear en el transversal.
- [[06 Líneas Múltiples (align-content)]] — alinear las líneas cuando hay wrap.
- [[07 Espaciado (gap)]] — el espacio entre items.
- [[08 order]] — reordenar visualmente.
- [[09 flex-grow, flex-shrink, flex-basis]] — cómo crecen y encogen los items.
- [[10 Casos de Uso]] — recetas habituales.

## El centrado perfecto

> [!tip] Centrar en ambos ejes con Flexbox
> El "santo grial" del centrado, trivial con Flexbox:
> ```css
> .centro {
>   display: flex;
>   justify-content: center;   /* centra en el eje principal */
>   align-items: center;       /* centra en el eje transversal */
> }
> ```
> Centra cualquier contenido (de tamaño desconocido) horizontal y verticalmente. Con `place-items: center` (Grid) o `place-content` aún más corto.

## Flexbox vs. Grid

Flexbox es **una** dimensión (reparte a lo largo de un eje, el contenido influye en el tamaño); [[04 CSS Grid/index | Grid]] es **dos** (rejilla explícita de filas y columnas). Para una barra o una fila de botones, Flexbox; para un layout de página o una rejilla, Grid. Se combinan constantemente.

## Notas relacionadas

- [[01 Contenedor Flex (display flex)]] — el punto de partida.
- [[10 Casos de Uso]] — recetas listas para copiar.
- [[04 CSS Grid/index]] — el sistema bidimensional, complementario.
