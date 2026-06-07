---
title: HWB — Tono, blancura y negrura
aliases:
  - hwb
tags:
  - css
  - api/funcion
  - fondos
draft: false
---

# HWB

> [!definicion]
> La función `hwb()` (Hue, Whiteness, Blackness) define un color por su **tono** y cuánto **blanco** y **negro** se le mezclan. Es un modelo pensado para ser intuitivo "como pintar": tomas un tono puro y lo aclaras con blanco o lo oscureces con negro.

```css
color: hwb(267 10% 5%);          /* tono 267°, 10% blanco, 5% negro */
color: hwb(267 10% 5% / 0.5);    /* con alfa */
```

## Los componentes

| Componente | Rango | Qué controla |
|------------|-------|--------------|
| Tono (hue) | 0–360 | El color base (igual que en HSL) |
| Blancura | 0–100% | Cuánto blanco se mezcla |
| Negrura | 0–100% | Cuánto negro se mezcla |

El tono es el mismo círculo cromático que [[04 HSL y HSLA | HSL]] (0 rojo, 120 verde, 240 azul).

## La intuición: mezclar pintura

> [!info] Como aclarar u oscurecer un pigmento
> HWB modela la forma en que se piensa el color al pintar: partes de un tono puro y le añades blanco (para pastel) o negro (para oscurecer):
> ```css
> hwb(267 0% 0%)     /* tono puro, vívido */
> hwb(267 50% 0%)    /* mucho blanco: pastel claro */
> hwb(267 0% 50%)    /* mucho negro: tono oscuro */
> hwb(267 40% 40%)   /* apagado, grisáceo */
> ```

## Cuando blanco + negro ≥ 100%

> [!info] Se normaliza a un gris
> Si la suma de blancura y negrura es **≥ 100%**, el tono desaparece y el resultado es un **gris**, cuyo nivel depende de la proporción entre ambos:
> ```css
> hwb(267 50% 50%)   /* gris medio (el tono no importa) */
> hwb(0 70% 70%)     /* gris, proporción 50/50 tras normalizar */
> ```

## Relación con HSL

HWB y HSL describen el mismo espacio sRGB con ejes distintos. HWB suele ser más natural para **aclarar/oscurecer** (añadir blanco/negro), mientras HSL es más natural para **saturar/desaturar**. Comparten la misma limitación: no son perceptualmente uniformes (ver [[06 LAB y LCH | OKLCH]]).

## Soporte y uso

> [!info] Menos extendido que HSL
> HWB tiene buen soporte en navegadores modernos, pero en la práctica se usa **menos** que HSL: cubre un nicho parecido y HSL está más arraigado. Conocerlo es útil; para la mayoría de casos, HSL u OKLCH son suficientes.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa HWB si piensas el color en términos de "tono + cuánto blanco/negro".
> - Para aclarar a pastel, sube la blancura; para oscurecer, la negrura.
> - Para contraste perceptual consistente, prefiere OKLCH.
> - Como siempre, define los colores en variables.

## Errores comunes

> [!warning] Trampas
> - **Esperar el tono** cuando blanco + negro ≥ 100%: sale gris.
> - **Olvidar los `%`** en blancura/negrura.
> - **Asumir uniformidad perceptual**: HWB tampoco la tiene.

## Notas relacionadas

- [[04 HSL y HSLA]] — el modelo intuitivo más usado.
- [[06 LAB y LCH]] — la opción perceptualmente uniforme.
- [[07 color-mix()]] — mezclar colores con blanco/negro de otra forma.
