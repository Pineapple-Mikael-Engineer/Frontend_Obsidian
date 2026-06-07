---
title: text-shadow — Sombra del texto
aliases:
  - text-shadow
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: text-shadow
grupo: tipografia
valor_inicial: none
hereda: true
animable: true
draft: false
---

# text-shadow

> [!definicion]
> `text-shadow` añade una o varias **sombras** al texto. Su uso más valioso no es decorativo sino **funcional**: mejorar el contraste de texto sobre fondos variables (una imagen), donde garantiza legibilidad. También sirve para efectos de profundidad, brillo (glow) y relieve.

```css
.sobre-imagen {
  text-shadow: 0 1px 3px rgb(0 0 0 / 0.6);   /* legibilidad sobre foto */
}
```

## Sintaxis

```css
text-shadow: <x> <y> <blur> <color>;
```

| Componente | Qué controla |
|------------|--------------|
| `<x>` | Desplazamiento horizontal (positivo = derecha) |
| `<y>` | Desplazamiento vertical (positivo = abajo) |
| `<blur>` | Radio de desenfoque (0 = nítida) |
| `<color>` | Color de la sombra |

```css
text-shadow: 2px 2px 4px rgb(0 0 0 / 0.5);
/*           x   y   blur color */
```

(A diferencia de [[03 Sombras (box-shadow) | `box-shadow`]], `text-shadow` **no** tiene spread ni `inset`.)

## Varias sombras

Separadas por comas, se apilan (la primera arriba). Permite efectos como contornos o brillos:

```css
/* Contorno simulado con 4 sombras */
.contorno {
  text-shadow:
    -1px -1px 0 #000, 1px -1px 0 #000,
    -1px  1px 0 #000, 1px  1px 0 #000;
}
```

## El uso estrella: legibilidad sobre imágenes

> [!tip] Texto legible sobre cualquier fondo
> Cuando hay texto sobre una imagen (un hero, un overlay), el fondo puede ser claro u oscuro según la zona, comprometiendo la legibilidad. Una `text-shadow` sutil y oscura crea un halo que **separa** el texto del fondo en cualquier caso:
> ```css
> .titulo-hero {
>   color: white;
>   text-shadow: 0 1px 4px rgb(0 0 0 / 0.7);
> }
> ```
> Es más robusto que confiar en que la imagen sea siempre oscura.

## Efectos de glow y relieve

```css
/* Brillo (neón) */
.neon { text-shadow: 0 0 8px #cba6f7, 0 0 16px #cba6f7; }

/* Relieve sutil (letterpress) */
.relieve { color: #ccc; text-shadow: 0 1px 0 rgb(255 255 255 / 0.5); }
```

## No abusar

> [!warning] La sombra puede emborronar el texto
> Sombras muy marcadas o con mucho desenfoque **reducen** la legibilidad en vez de mejorarla, y dan un aspecto anticuado. Para texto de lectura, una sombra debe ser **sutil** (poco desplazamiento, opacidad moderada) o no estar. Reserva los efectos llamativos (neón, relieve fuerte) para titulares y elementos decorativos puntuales.

## Buenas prácticas

> [!tip] Recomendaciones
> - Úsala sobre todo para **legibilidad** sobre fondos variables (imágenes).
> - Sombras sutiles en texto de lectura; efectos fuertes solo en titulares.
> - Varias sombras para contornos o brillos.
> - Usa `rgb(... / alfa)` para sombras semitransparentes naturales.

## Errores comunes

> [!warning] Trampas
> - **Sombra demasiado fuerte**: emborrona y resta legibilidad.
> - **Confundir con `box-shadow`**: `text-shadow` no tiene spread ni `inset`.
> - **Abusar de efectos** (neón, relieve) en texto de cuerpo.

## Notas relacionadas

- [[03 Sombras (box-shadow)]] — sombras de cajas (con spread e inset).
- [[01 color]] — el contraste del texto que la sombra refuerza.
- [[02 background-image]] — los fondos sobre los que la sombra da legibilidad.
