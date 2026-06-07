---
title: box-shadow — Sombras de la caja
aliases:
  - box-shadow
tags:
  - css
  - api/propiedad
  - fondos
propiedad: box-shadow
grupo: fondo
valor_inicial: none
hereda: false
animable: true
draft: false
---

# box-shadow

> [!definicion]
> `box-shadow` añade una o varias **sombras** a la caja de un elemento, dando sensación de **profundidad y elevación**. A diferencia de [[01 text-shadow | `text-shadow`]] (que sombrea el texto), `box-shadow` afecta a toda la caja, y admite **spread** (expansión) y sombras **internas** (`inset`).

```css
.tarjeta { box-shadow: 0 4px 12px rgb(0 0 0 / 0.15); }
```

## Sintaxis

```css
box-shadow: <x> <y> <blur> <spread> <color>;
```

| Componente | Controla |
|------------|----------|
| `<x>` | Desplazamiento horizontal (+ derecha) |
| `<y>` | Desplazamiento vertical (+ abajo) |
| `<blur>` | Radio de desenfoque (mayor = más difusa) |
| `<spread>` | Expansión/contracción de la sombra (opcional) |
| `<color>` | Color (idealmente con alfa) |
| `inset` | Hace la sombra **interna** (hacia dentro) |

```css
box-shadow: 0 4px 8px 0 rgb(0 0 0 / 0.2);
/*          x y  blur spread color */
```

## La elevación (Material Design)

> [!tip] Sombras que comunican jerarquía
> Las sombras transmiten **elevación**: cuanto más "alta" está una superficie, mayor y más difusa es su sombra. Una escala de sombras coherente da jerarquía visual:
> ```css
> :root {
>   --sombra-sm: 0 1px 2px rgb(0 0 0 / 0.1);
>   --sombra-md: 0 4px 8px rgb(0 0 0 / 0.12);
>   --sombra-lg: 0 12px 24px rgb(0 0 0 / 0.15);
> }
> .tarjeta { box-shadow: var(--sombra-md); }
> .tarjeta:hover { box-shadow: var(--sombra-lg); }   /* "se levanta" */
> ```

## Sombras realistas: usa varias capas

> [!tip] Sombras en capas para más realismo
> Una sola sombra se ve plana. Las sombras realistas apilan **varias** con distinto desenfoque y opacidad (las sombras reales tienen un núcleo nítido y un halo difuso):
> ```css
> box-shadow:
>   0 1px 2px rgb(0 0 0 / 0.1),
>   0 4px 8px rgb(0 0 0 / 0.08),
>   0 16px 24px rgb(0 0 0 / 0.06);
> ```
> Esta técnica ("layered shadows") da un resultado mucho más natural que una sombra única y fuerte.

## inset: sombras internas

`inset` proyecta la sombra **hacia dentro**, útil para hundir un elemento, simular un campo de texto, o un efecto de presionado:

```css
.input { box-shadow: inset 0 2px 4px rgb(0 0 0 / 0.1); }
.btn:active { box-shadow: inset 0 2px 6px rgb(0 0 0 / 0.2); }  /* presionado */
```

## spread y los bordes/glow

El `spread` expande (positivo) o contrae (negativo) la sombra. Con desenfoque 0, crea un **borde** sólido (alternativa a `border` que no ocupa espacio); con color vivo, un **glow**:

```css
.focus-ring { box-shadow: 0 0 0 3px rgb(203 166 247 / 0.5); }  /* anillo de foco */
.neon { box-shadow: 0 0 12px #cba6f7, 0 0 24px #cba6f7; }      /* brillo */
```

## La sombra no ocupa espacio

> [!info] box-shadow no afecta al layout
> A diferencia del `border`, la sombra **no ocupa espacio** ni desplaza otros elementos: se pinta "fuera" del flujo. Por eso aparecer/desaparecer una sombra en `:hover` no causa saltos de layout (a diferencia de añadir un borde). Esto la hace ideal para efectos de hover.

## Rendimiento

> [!warning] Sombras grandes y animadas son caras
> Sombras con mucho desenfoque o spread, sobre todo si se **animan**, tienen coste de repintado. Para animar elevación de forma fluida, una técnica es tener la sombra en un pseudoelemento y animar su `opacity` (más barato que animar `box-shadow`). Detalle en [[03 Animar transform y opacity]].

## Buenas prácticas

> [!tip] Recomendaciones
> - Define una **escala** de sombras en variables (sm/md/lg) para jerarquía coherente.
> - Apila varias sombras sutiles para realismo, en vez de una fuerte.
> - Usa el patrón `spread` con desenfoque 0 para anillos de foco y bordes sin ocupar espacio.
> - Anima `opacity` de una sombra en pseudoelemento, no el `box-shadow`, para fluidez.

## Errores comunes

> [!warning] Trampas
> - **Una sombra única muy fuerte**: se ve plana y anticuada.
> - **Sombras negras opacas** (`#000`): poco naturales; usa negro con alfa.
> - **Animar `box-shadow` pesado**: tirones; anima opacidad en su lugar.

## Notas relacionadas

- [[01 text-shadow]] — sombras del texto (sin spread ni inset).
- [[05 border-radius]] — la sombra sigue las esquinas redondeadas.
- [[03 Animar transform y opacity]] — animar elevación de forma fluida.
