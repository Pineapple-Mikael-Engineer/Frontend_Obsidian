---
title: border-image — Usar una imagen como borde
aliases:
  - border-image
tags:
  - css
  - api/propiedad
  - layout
propiedad: border-image
grupo: box-model
valor_inicial: none
hereda: false
animable: false
draft: false
---

# border-image

> [!definicion]
> `border-image` usa una **imagen** (o un gradiente) como borde de la caja, en lugar de una línea de color. Reparte la imagen en nueve zonas (las cuatro esquinas, los cuatro lados y el centro) y la "estira" o "repite" para adaptarse al tamaño. Es una propiedad poderosa pero de nicho.

```css
.marco {
  border: 30px solid transparent;
  border-image: url("marco.png") 30 round;
}
```

## El modelo de las 9 zonas (9-slice)

> [!info] Cómo se trocea la imagen
> `border-image` divide la imagen de origen con cuatro cortes (`border-image-slice`), creando una rejilla 3×3:
> - Las **4 esquinas** se colocan en las esquinas (sin estirar).
> - Los **4 lados** se estiran o repiten a lo largo de cada borde.
> - El **centro** se descarta por defecto (o se usa de fondo con `fill`).
>
> Esto permite marcos decorativos que escalan a cualquier tamaño sin deformar las esquinas, la misma técnica de los "9-slice scaling" de los videojuegos.

## Las sub-propiedades

| Propiedad | Controla |
|-----------|----------|
| `border-image-source` | La imagen o gradiente de origen |
| `border-image-slice` | Dónde cortar la imagen (las 4 líneas) |
| `border-image-width` | El grosor de la imagen del borde |
| `border-image-outset` | Cuánto sobresale la imagen del borde |
| `border-image-repeat` | `stretch`, `repeat`, `round`, `space` para los lados |

## Requiere un border de respaldo

> [!warning] Necesita border-width o border con grosor
> `border-image` se dibuja **sobre** el área del borde, así que la caja debe tener un `border` con grosor (aunque sea transparente) para reservar el espacio:
> ```css
> .x {
>   border: 30px solid transparent;   /* reserva el área */
>   border-image: url("marco.png") 30 round;
> }
> ```
> Sin el `border-width`, no hay dónde pintar la imagen y no se ve.

## Gradientes como borde

Un uso moderno y práctico: bordes con **gradiente**, imposibles con `border-color` (que solo acepta colores planos):

```css
.borde-degradado {
  border: 4px solid transparent;
  border-image: linear-gradient(45deg, #cba6f7, #89dceb) 1;
}
```

> [!warning] border-image no respeta border-radius
> Una limitación importante: `border-image` **ignora** [[05 border-radius | `border-radius`]] — las esquinas redondeadas no funcionan con bordes de imagen/gradiente. Para un borde degradado **con** esquinas redondeadas, se usan otras técnicas (un fondo con `background-clip`, o `mask`).

## Buenas prácticas

> [!tip] Recomendaciones
> - Reserva el espacio con un `border` de grosor (transparente) antes del `border-image`.
> - Para bordes degradados simples, `border-image: linear-gradient(...) 1`.
> - Si necesitas borde degradado **y** esquinas redondeadas, usa `background`/`mask`, no `border-image`.
> - Es una propiedad de nicho: para bordes normales, basta el [[04 Shorthand border | shorthand `border`]].

## Errores comunes

> [!warning] Trampas
> - **Sin `border-width`**: la imagen no tiene dónde dibujarse.
> - **Esperar `border-radius`**: `border-image` lo ignora.
> - **`slice` mal calculado**: las esquinas se deforman o se cortan.

## Notas relacionadas

- [[04 Shorthand border]] — el borde normal de color.
- [[05 border-radius]] — esquinas redondeadas (incompatibles con border-image).
- [[01 linear-gradient]] — gradientes, usables como border-image.
