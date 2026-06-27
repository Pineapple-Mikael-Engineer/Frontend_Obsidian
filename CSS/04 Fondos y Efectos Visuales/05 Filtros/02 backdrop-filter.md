---
title: backdrop-filter — Filtro sobre el fondo (cristal esmerilado)
aliases:
  - backdrop-filter
  - glassmorphism
tags:
  - css
  - api/propiedad
  - fondos
propiedad: backdrop-filter
grupo: fondo
valor_inicial: none
hereda: false
animable: true
draft: false
order: 2
---

# backdrop-filter

> [!definicion]
> `backdrop-filter` aplica un filtro a **lo que hay detrás** de un elemento (no al elemento mismo). Su uso estrella es el **cristal esmerilado** (glassmorphism): un panel semitransparente que **desenfoca** el contenido que pasa por detrás, como una barra de navegación translúcida o una tarjeta de cristal.

```css
.barra {
  background: rgb(255 255 255 / 0.6);
  backdrop-filter: blur(10px);
}
```

## El efecto de cristal esmerilado

> [!tip] La receta del glassmorphism
> El efecto "vidrio" combina tres cosas: un fondo **semitransparente**, un **desenfoque** del fondo y, a menudo, un borde sutil:
> ```css
> .cristal {
>   background: rgb(255 255 255 / 0.15);   /* casi transparente */
>   backdrop-filter: blur(12px) saturate(1.5);
>   border: 1px solid rgb(255 255 255 / 0.2);
>   border-radius: 1rem;
> }
> ```
> El `blur` difumina lo que pasa por detrás; la transparencia deja entrever el color; el `saturate` aviva los colores del fondo. Es el look de las interfaces de iOS/macOS.

## filter vs. backdrop-filter

> [!info] El elemento mismo vs. lo que tiene detrás
> La diferencia clave con [[01 filter | `filter`]]:
> - **`filter: blur()`** desenfoca **el elemento y su contenido** (el texto del panel se vería borroso).
> - **`backdrop-filter: blur()`** desenfoca **lo que está detrás**, dejando el contenido del panel **nítido**.
>
> Por eso el cristal usa `backdrop-filter`: el fondo se difumina, pero el texto encima se lee perfectamente.

## Acepta las mismas funciones que filter

`backdrop-filter` admite las mismas funciones que `filter` (`blur`, `brightness`, `saturate`, `contrast`…), encadenables:

```css
backdrop-filter: blur(8px) brightness(0.9);
```

## Casos de uso

```css
/* Barra de navegación translúcida pegajosa */
.nav {
  position: sticky; top: 0;
  background: rgb(30 30 46 / 0.7);
  backdrop-filter: blur(10px);
}

/* Modal con fondo desenfocado */
.overlay-modal { backdrop-filter: blur(4px) brightness(0.8); }

/* Tarjeta de cristal sobre una imagen */
.glass-card {
  background: rgb(255 255 255 / 0.1);
  backdrop-filter: blur(16px);
}
```

## Rendimiento y respaldo

> [!warning] backdrop-filter es de los efectos más caros
> Repintar y desenfocar el fondo **en tiempo real** es costoso, sobre todo si el elemento es grande, se mueve o hay varios. En móvil puede afectar a la fluidez. Recomendaciones:
> - Úsalo en pocos elementos (una barra, un modal), no en muchas tarjetas a la vez.
> - Da un **respaldo**: un `background` más opaco para navegadores sin soporte (con `@supports`):
> ```css
> .nav { background: rgb(30 30 46 / 0.95); }            /* respaldo opaco */
> @supports (backdrop-filter: blur(10px)) {
>   .nav { background: rgb(30 30 46 / 0.7); backdrop-filter: blur(10px); }
> }
> ```
> - Puede necesitar el prefijo `-webkit-backdrop-filter` en algunos navegadores.

## Buenas prácticas

> [!tip] Recomendaciones
> - Combina fondo semitransparente + `blur` para el efecto de cristal.
> - Úsalo en pocos elementos clave (barra, modal), no masivamente.
> - Da un respaldo opaco con `@supports` para navegadores sin soporte.
> - Mide el rendimiento, especialmente en móvil.

## Errores comunes

> [!warning] Trampas
> - **Usar `filter` en vez de `backdrop-filter`**: desenfoca el contenido del panel, no el fondo.
> - **Fondo totalmente opaco**: el `blur` no se ve (no hay transparencia que deje pasar el fondo).
> - **Muchos elementos con `backdrop-filter`**: tirones.
> - **Sin respaldo**: el panel queda ilegible (transparente) donde no hay soporte.

## Notas relacionadas

- [[01 filter]] — el filtro sobre el propio elemento.
- [[01 background-color]] — el fondo semitransparente del cristal.
- [[06 Preferencias (prefers-color-scheme, prefers-reduced-motion)]] — `@supports` y feature queries.
