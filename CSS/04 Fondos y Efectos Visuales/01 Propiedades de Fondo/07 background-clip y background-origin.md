---
title: background-clip y background-origin — Hasta dónde llega el fondo
aliases:
  - background-clip
  - background-origin
tags:
  - css
  - api/propiedad
  - fondos
propiedad: background-clip
grupo: fondo
valor_inicial: border-box
hereda: false
animable: false
draft: false
---

# background-clip y background-origin

> [!definicion]
> `background-clip` define **hasta dónde se pinta** el fondo (recorte): bajo el borde, hasta el padding, o solo el contenido. `background-origin` define **desde dónde empieza** a posicionarse la imagen. Ambas usan las áreas del [[01 Partes del Modelo de Caja | modelo de caja]] como referencia.

```css
.recortado { background-clip: padding-box; }   /* el fondo no llega bajo el borde */
```

## Los valores (las áreas de la caja)

| Valor | El fondo llega hasta… |
|-------|------------------------|
| `border-box` | El borde exterior (por defecto en `clip`) |
| `padding-box` | El borde interior (bajo el padding, no el borde) |
| `content-box` | Solo el área de contenido |
| `text` | **El texto** (solo `background-clip`): recorta el fondo a la forma de las letras |

## background-clip: text, el efecto estrella

> [!tip] Texto con relleno de gradiente
> El valor `background-clip: text` recorta el fondo a la **forma del texto**, creando letras "rellenas" de un gradiente o una imagen. Es un efecto muy popular para titulares llamativos:
> ```css
> .titulo-degradado {
>   background: linear-gradient(135deg, #cba6f7, #f38ba8);
>   background-clip: text;
>   -webkit-background-clip: text;   /* aún necesita prefijo */
>   color: transparent;              /* oculta el color sólido para ver el fondo */
> }
> ```
> El truco: el texto se hace transparente (`color: transparent`) para que se vea el gradiente recortado a su forma. Aún requiere el prefijo `-webkit-`.

## background-origin: el punto de partida

`background-origin` decide desde qué área se calcula la **posición** de la imagen (de dónde es el "0 0"):

```css
.con-padding {
  padding: 2rem;
  background-image: url("icono.svg");
  background-origin: content-box;   /* posición 0 0 = inicio del contenido, no del padding */
}
```

Por defecto es `padding-box`: la imagen se posiciona desde el borde del padding.

## clip vs. origin: la diferencia

> [!info] Recortar vs. posicionar
> Es fácil confundirlas:
> - **`background-clip`**: dónde se **recorta** (hasta dónde se ve) el fondo. Afecta a lo que se pinta.
> - **`background-origin`**: desde dónde se **mide** la posición de la imagen. Afecta a dónde se ancla.
>
> Una controla el "hasta dónde llega", la otra el "desde dónde empieza".

## Caso útil: borde semitransparente

`background-clip: padding-box` evita que el fondo se vea **a través** de un borde semitransparente o punteado, un detalle visual fino:

```css
.borde-translucido {
  border: 4px dashed rgb(0 0 0 / 0.3);
  background: #cba6f7;
  background-clip: padding-box;   /* el fondo no asoma por los huecos del borde */
}
```

## Buenas prácticas

> [!tip] Recomendaciones
> - `background-clip: text` (con `-webkit-` y `color: transparent`) para titulares con gradiente.
> - `padding-box` para que el fondo no asome por bordes punteados/translúcidos.
> - `background-origin: content-box` cuando la imagen deba posicionarse desde el contenido, no el padding.
> - Da un color de respaldo si usas `clip: text` (por si el efecto no aplica).

## Errores comunes

> [!warning] Trampas
> - **`background-clip: text` sin `color: transparent`**: el color sólido tapa el gradiente.
> - **Olvidar `-webkit-background-clip`**: el efecto de texto no se ve.
> - **Confundir `clip` con `origin`**: una recorta, la otra posiciona.

## Notas relacionadas

- [[01 Partes del Modelo de Caja]] — las áreas (border/padding/content) que usan de referencia.
- [[01 linear-gradient]] — el gradiente que suele rellenar el texto.
- [[08 Shorthand background]] — `clip`/`origin` en el shorthand.
