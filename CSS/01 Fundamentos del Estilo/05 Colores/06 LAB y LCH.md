---
title: LAB, LCH y OKLCH — Color perceptualmente uniforme
aliases:
  - lab
  - lch
  - oklch
  - oklab
tags:
  - css
  - api/funcion
  - fondos
draft: false
---

# LAB y LCH

> [!definicion]
> `lab()`, `lch()` y sus variantes modernas `oklab()`/`oklch()` definen color en espacios **perceptualmente uniformes** y de **amplio gamut**: a igual luminosidad, los colores se perciben igual de claros, y pueden representar tonos **más vivos** que sRGB. Son la base del color CSS moderno.

```css
color: oklch(0.8 0.12 300);          /* luz 0.8, croma 0.12, tono 300° */
color: oklch(0.8 0.12 300 / 0.5);    /* con alfa */
color: lab(75% 20 -50);
```

## Las cuatro funciones

| Función | Modelo | Componentes |
|---------|--------|-------------|
| `lab()` | Cartesiano | Luminosidad, eje a (verde↔rojo), eje b (azul↔amarillo) |
| `lch()` | Polar | Luminosidad, croma (saturación), tono (ángulo) |
| `oklab()` | Cartesiano mejorado | Como lab, con mejor uniformidad |
| `oklch()` | Polar mejorado | Como lch, con mejor uniformidad |

> [!tip] OKLCH es la recomendada
> De las cuatro, **`oklch()`** es la más práctica: combina la intuición polar de LCH (luminosidad, croma, tono — parecido a HSL) con la uniformidad mejorada de la familia "OK". Es la que se recomienda para diseño nuevo.

## Los componentes de OKLCH

| Componente | Rango | Qué controla |
|------------|-------|--------------|
| Luminosidad | 0–1 (o 0–100%) | Brillo percibido: 0 negro, 1 blanco |
| Croma | 0 a ~0.4 | Intensidad/saturación: 0 gris, alto vívido |
| Tono | 0–360 | El color en la rueda |

## La ventaja 1: uniformidad perceptual

> [!info] Igual luminosidad = igual brillo percibido
> El gran defecto de [[04 HSL y HSLA | HSL]] era que dos colores con la misma `lightness` se veían con brillo distinto (un amarillo y un azul al 50%). En OKLCH, la luminosidad es **perceptual**: dos colores con la misma `L` se perciben **igual de claros**, sin importar el tono. Esto permite:
> - Generar paletas con **contraste consistente**.
> - Rotar el tono manteniendo el brillo (temas de color que "funcionan" en cualquier tono).
> - Cumplir contraste de accesibilidad de forma más predecible.

## La ventaja 2: amplio gamut

> [!info] Colores más vivos que sRGB
> `oklch()` puede expresar colores fuera de sRGB, en el gamut **display-p3** (y más amplios), que las pantallas modernas muestran. Son rojos, verdes y azules **más saturados** de los que `#ff0000` puede dar. En pantallas que no lo soportan, el navegador hace *gamut mapping* al color más cercano representable.

## Generar una paleta con OKLCH

```css
:root {
  --acento: oklch(0.7 0.15 300);
  /* variantes manteniendo croma y tono, variando solo la luz */
  --acento-claro:  oklch(0.85 0.15 300);
  --acento-oscuro: oklch(0.55 0.15 300);
}
```

Como la luz es perceptual, las tres variantes tienen una relación de brillo predecible.

## Soporte

> [!info] Soporte moderno, amplio
> `oklch()`/`oklab()` tienen buen soporte en navegadores actuales. Para navegadores muy antiguos, se puede ofrecer un respaldo en hex/`rgb()` antes de la declaración OKLCH (el navegador usa la última que entiende):
> ```css
> color: #b07fd9;                 /* respaldo */
> color: oklch(0.7 0.15 300);     /* moderno, gana donde se soporta */
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Para diseño nuevo, usa **OKLCH** como formato principal: paletas consistentes y colores vivos.
> - Genera variantes (hover, estados) variando solo la luminosidad.
> - Aprovecha la uniformidad para garantizar contraste accesible.
> - Da un respaldo hex/rgb si necesitas soportar navegadores antiguos.

## Errores comunes

> [!warning] Trampas
> - **Croma demasiado alto**: fuera de gamut, el navegador lo recorta al máximo representable.
> - **Confundir la `L` de OKLCH (0–1) con la de HSL (0–100%)**: escalas distintas.
> - **No dar respaldo** en proyectos que aún soportan navegadores viejos.

## Notas relacionadas

- [[04 HSL y HSLA]] — el modelo intuitivo pero no perceptual.
- [[07 color-mix()]] — mezclar colores, idealmente en espacio OKLCH.
- [[10 Variables CSS/index]] — definir paletas OKLCH reutilizables.
