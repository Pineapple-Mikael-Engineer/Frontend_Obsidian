---
title: aspect-ratio — Mantener una proporción
aliases:
  - aspect-ratio
tags:
  - css
  - api/propiedad
  - layout
propiedad: aspect-ratio
grupo: box-model
valor_inicial: auto
hereda: false
animable: false
draft: false
---

# aspect-ratio

> [!definicion]
> `aspect-ratio` fija la **proporción** entre el ancho y el alto de una caja. Si solo se define una dimensión (o el ancho es fluido), la otra se calcula automáticamente para mantener la proporción. Sustituye el viejo "truco del padding" por una propiedad directa.

```css
.video { aspect-ratio: 16 / 9; }   /* el alto se deriva del ancho */
.cuadro { aspect-ratio: 1; }       /* cuadrado */
```

## Sintaxis

| Valor | Significado |
|-------|-------------|
| `16 / 9` | Proporción ancho/alto (panorámico) |
| `4 / 3` | Proporción clásica |
| `1` (o `1 / 1`) | Cuadrado |
| `auto` | Sin proporción forzada (por defecto) |

## Cómo funciona con width/height

> [!info] Una dimensión define, la otra se calcula
> `aspect-ratio` actúa cuando **una** dimensión está determinada y la otra es `auto`:
> - Con `width: 100%` y `aspect-ratio: 16/9`, el **alto** se calcula como `ancho × 9/16`.
> - Si fijas **ambas** (`width` y `height`), `aspect-ratio` se ignora (las dimensiones explícitas mandan).
> ```css
> .tarjeta-img {
>   width: 100%;
>   aspect-ratio: 3 / 2;   /* el alto sigue al ancho, proporción 3:2 */
> }
> ```

## El reemplazo del truco del padding

> [!tip] Antes había que usar padding-top: 56.25%
> Mantener una proporción solía hacerse con el [[05 Porcentajes | truco de `padding-top`]] (un porcentaje calculado sobre el ancho) más un hijo posicionado en absoluto. Era enrevesado. `aspect-ratio` lo hace en una línea, sin posicionamiento ni cálculos:
> ```css
> /* viejo */ .v { padding-top: 56.25%; position: relative; }
> /* nuevo */ .v { aspect-ratio: 16 / 9; }
> ```

## Casos de uso

| Caso | Uso |
|------|-----|
| Vídeos / iframes responsivos | `aspect-ratio: 16 / 9` |
| Miniaturas e imágenes con proporción uniforme | `aspect-ratio: 1` u otra |
| Tarjetas con imagen de cabecera consistente | `aspect-ratio` + `object-fit: cover` |
| Placeholders que reservan espacio antes de cargar | evita saltos de layout |

## Con imágenes y object-fit

Para que una imagen llene una caja con proporción fija sin deformarse, se combina con `object-fit`:

```css
.miniatura {
  width: 100%;
  aspect-ratio: 1;
  object-fit: cover;   /* recorta para llenar sin deformar */
}
```

## El contenido puede romper la proporción

> [!warning] El contenido puede forzar más altura
> Si el **contenido** de una caja con `aspect-ratio` necesita más alto del que la proporción daría, la caja **crece** para no cortarlo (el `aspect-ratio` cede ante el contenido, salvo que limites con `overflow` o `min-height`). Por eso funciona mejor en cajas cuyo tamaño no depende de texto variable (imágenes, vídeos, placeholders).

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `aspect-ratio` para vídeos, imágenes y placeholders con proporción fija.
> - Combínalo con `object-fit: cover` en imágenes para llenar sin deformar.
> - Define solo una dimensión (normalmente `width`) y deja que la otra se calcule.
> - Reserva el espacio antes de cargar imágenes para evitar saltos de layout.

## Errores comunes

> [!warning] Trampas
> - **Fijar `width` y `height` a la vez**: `aspect-ratio` se ignora.
> - **Esperar que recorte texto**: el contenido puede romper la proporción.
> - **Olvidar `object-fit`** en imágenes: se deforman al forzar la proporción.

## Notas relacionadas

- [[01 width y height]] — la dimensión que `aspect-ratio` complementa.
- [[05 Porcentajes]] — el viejo truco que reemplaza.
- [[01 Imágenes Responsivas (picture, source)]] — imágenes con proporción.
