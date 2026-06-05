---
title: HSL y HSLA — Tono, saturación y luminosidad
aliases:
  - hsl
  - hsla
tags:
  - css
  - api/funcion
  - fondos
draft: false
---

# HSL y HSLA

> [!definicion]
> La función `hsl()` define un color por **tono** (hue), **saturación** y **luminosidad**, un modelo mucho más **intuitivo** para los humanos que RGB: permite ajustar una sola dimensión (más claro, menos saturado) sin tocar las demás. El cuarto valor opcional es el alfa.

```css
color: hsl(267 89% 81%);          /* tono 267°, 89% sat, 81% luz */
color: hsl(267 89% 81% / 0.5);    /* con alfa */
```

## Los tres componentes

| Componente | Rango | Qué controla |
|------------|-------|--------------|
| Tono (hue) | 0–360 (grados) | El color en la rueda cromática |
| Saturación | 0–100% | Intensidad: 0% gris, 100% vívido |
| Luminosidad | 0–100% | Brillo: 0% negro, 50% normal, 100% blanco |

### El tono es una rueda

El tono es un ángulo en el círculo cromático:

| Grados | Color |
|--------|-------|
| 0 / 360 | Rojo |
| 60 | Amarillo |
| 120 | Verde |
| 180 | Cian |
| 240 | Azul |
| 300 | Magenta |

## Por qué es tan práctico

> [!tip] Ajustar una dimensión a la vez
> La gran ventaja de HSL: para variar un color, tocas **un** valor:
> ```css
> --base: hsl(267 89% 81%);
> /* más oscuro: baja la luminosidad */
> --oscuro: hsl(267 89% 60%);
> /* desaturado: baja la saturación */
> --apagado: hsl(267 40% 81%);
> ```
> Esto hace HSL ideal para **generar paletas** (un mismo tono en varias luminosidades) y para estados (hover más oscuro), algo casi imposible de hacer a mano en RGB/hex.

## Generar una escala de grises

Con saturación 0%, el tono es irrelevante y solo cuenta la luminosidad — una forma limpia de hacer grises:

```css
hsl(0 0% 100%)   /* blanco */
hsl(0 0% 90%)    /* gris muy claro */
hsl(0 0% 50%)    /* gris medio */
hsl(0 0% 10%)    /* casi negro */
```

## La limitación: no es perceptualmente uniforme

> [!warning] La misma luminosidad no se ve igual de clara
> El gran defecto de HSL: su "luminosidad" es matemática, no perceptual. Dos colores con la **misma** `lightness` (p. ej. amarillo y azul al 50%) se perciben con **brillo muy distinto** (el amarillo parece mucho más claro). Esto complica crear paletas con contraste consistente. La solución moderna es [[06 LAB y LCH | OKLCH]], que sí es perceptualmente uniforme: igual luminosidad = igual brillo percibido.

## Sintaxis moderna y heredada

```css
hsl(267 89% 81% / 0.5)        /* moderno: espacios + / */
hsla(267, 89%, 81%, 0.5)      /* heredado: comas */
```

Como con RGB, `hsl()` moderno ya admite alfa, así que `hsla()` es redundante.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa HSL para **ajustar** colores y **generar paletas** (variar tono/sat/luz por separado).
> - Saturación 0% para grises neutros controlados por luminosidad.
> - Para contraste perceptualmente consistente entre tonos distintos, prefiere OKLCH.
> - Define el tono base como variable y deriva variantes con HSL.

## Errores comunes

> [!warning] Trampas
> - **Olvidar el `%`** en saturación/luminosidad: son porcentajes obligatorios.
> - **Esperar brillo uniforme** entre tonos a igual `lightness`: HSL no es perceptual.
> - **El tono fuera de 0–360**: se normaliza (370 = 10), pero confunde.

## Notas relacionadas

- [[03 RGB y RGBA]] — el modelo por canales, menos intuitivo de ajustar.
- [[05 HWB]] — otra forma intuitiva (tono + blanco + negro).
- [[06 LAB y LCH]] — la alternativa perceptualmente uniforme.
