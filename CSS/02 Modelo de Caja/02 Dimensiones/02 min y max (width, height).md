---
title: min y max (width, height) — Acotar el tamaño
aliases:
  - min-width
  - max-width
  - min-height
  - max-height
tags:
  - css
  - api/propiedad
  - layout
propiedad: max-width
grupo: box-model
valor_inicial: none
hereda: false
animable: true
draft: false
---

# min y max (width, height)

> [!definicion]
> `min-width`, `max-width`, `min-height` y `max-height` **acotan** las dimensiones de una caja entre límites. Permiten que un elemento sea fluido (crece y encoge) pero sin pasarse de un mínimo o un máximo. Son la base del diseño responsivo sin media queries.

```css
.contenedor {
  width: 100%;
  max-width: 65rem;    /* nunca más ancho que esto */
  min-width: 20rem;    /* nunca más estrecho */
}
```

## Las cuatro propiedades

| Propiedad | Efecto | Valor inicial |
|-----------|--------|---------------|
| `min-width` | Ancho mínimo (la caja no encoge más) | `auto` |
| `max-width` | Ancho máximo (la caja no crece más) | `none` |
| `min-height` | Alto mínimo | `auto` |
| `max-height` | Alto máximo | `none` |

## max-width: el más útil

> [!tip] max-width hace fluido con tope
> `max-width` es la propiedad de dimensión más usada en diseño responsivo. Combinada con un ancho fluido, da el patrón "ocupa lo disponible pero no te estires":
> ```css
> .lectura { width: 100%; max-width: 40rem; margin-inline: auto; }
> ```
> En móvil, ocupa el 100%; en escritorio, se limita a 40rem y se centra. **Una** línea sustituye varias media queries.

## La prioridad: min gana a max gana a width

> [!info] El orden de resolución
> Cuando hay conflicto, la prioridad es: **`min` > `max` > `width`**. Es decir:
> 1. El navegador toma `width` (o `auto`).
> 2. Lo recorta a `max-width` si lo supera.
> 3. Lo expande a `min-width` si no llega.
>
> Por eso, si `min-width` > `max-width`, **gana `min-width`** (el mínimo siempre se respeta). Esto evita cajas imposibles.

## min-height vs. height

`min-height` es casi siempre preferible a `height` para contenedores: garantiza una altura mínima pero **deja crecer** el contenido, evitando los desbordamientos de una `height` fija:

```css
.hero { min-height: 100dvh; }   /* al menos toda la pantalla, más si el contenido lo pide */
```

## max-height y el contenido oculto

`max-height` limita el alto, pero el contenido que sobra **desborda** salvo que se controle con `overflow`. Es la base de patrones de "ver más" o paneles colapsables (a menudo animando `max-height`, aunque tiene limitaciones):

```css
.panel { max-height: 0; overflow: hidden; transition: max-height 0.3s; }
.panel.abierto { max-height: 30rem; }
```

## min-width: auto en flex/grid

> [!warning] El min-width automático que causa desbordamientos
> Un detalle avanzado pero importante: en items de **Flexbox/Grid**, `min-width` (y `min-height`) valen `auto` por defecto, lo que impide que el item encoja por debajo del tamaño de su contenido. Esto causa el clásico "un item desborda el contenedor flex". La solución es `min-width: 0` en el item:
> ```css
> .item-flex { min-width: 0; }   /* permite encoger por debajo del contenido */
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - `max-width` en `rem` + ancho fluido = contenedor responsivo sin media queries.
> - `min-height` en vez de `height` para no cortar contenido.
> - Recuerda la prioridad: `min` gana a `max` gana a `width`.
> - `min-width: 0` en items flex/grid que desbordan por su contenido.

## Errores comunes

> [!warning] Trampas
> - **`min-width` > `max-width`**: gana el mínimo (puede no ser lo esperado).
> - **Item flex que desborda**: por el `min-width: auto` implícito; pon `min-width: 0`.
> - **`max-height` sin `overflow`**: el contenido sobrante se sale.

## Notas relacionadas

- [[01 width y height]] — el tamaño base que estas acotan.
- [[07 min(), max(), clamp()]] — acotar dentro de un único valor.
- [[09 flex-grow, flex-shrink, flex-basis]] — el encogimiento en Flexbox.
