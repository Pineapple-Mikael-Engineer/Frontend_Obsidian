---
title: Hexadecimal — Color en base 16 (#rrggbb)
aliases:
  - hex color
  - hexadecimal
tags:
  - css
  - api/concepto
  - fondos
draft: false
order: 2
---

# Hexadecimal

> [!definicion]
> El formato **hexadecimal** expresa un color como tres pares de dígitos en base 16 precedidos de `#`: rojo, verde y azul, cada uno de `00` a `ff` (0–255). Es el formato más usado de la web por su concisión y soporte universal.

```css
color: #cba6f7;   /* R=cb(203) G=a6(166) B=f7(247) */
background: #1e1e2e;
```

## Cómo se lee

Cada par de dígitos es un canal en base 16:

```
#cba6f7
 │ │ │
 │ │ └ azul:  f7 = 247
 │ └ verde: a6 = 166
 └ rojo:  cb = 203
```

Los dígitos hex van de `0` a `f`: `0123456789abcdef`. `ff` es 255 (máximo), `00` es 0 (mínimo). `#ffffff` es blanco, `#000000` negro, `#ff0000` rojo puro.

## Las formas: 3, 4, 6 y 8 dígitos

| Forma | Significado | Ejemplo |
|-------|-------------|---------|
| `#rgb` (3) | Cada dígito se duplica | `#f0a` = `#ff00aa` |
| `#rgba` (4) | 3 + alfa abreviado | `#f0a8` |
| `#rrggbb` (6) | Forma completa | `#ff00aa` |
| `#rrggbbaa` (8) | Completa + alfa | `#ff00aa80` |

> [!info] La forma corta duplica cada dígito
> `#f0a` no es lo mismo que `#f00a0a`: cada dígito se **duplica**, dando `#ff00aa`. Por eso la forma de 3 dígitos solo puede expresar 4096 colores (los que tienen pares repetidos). `#abc` = `#aabbcc`. Útil para colores "redondos" pero no para tonos precisos.

## El canal alfa en hex

Los dos últimos dígitos de la forma de 8 (o el cuarto de la de 4) controlan la **opacidad**:

| Alfa hex | Opacidad |
|----------|----------|
| `ff` | 100 % (opaco) |
| `cc` | ~80 % |
| `80` | ~50 % |
| `00` | 0 % (transparente) |

```css
background: #1e1e2e80;   /* el fondo de marca al 50 % */
```

Calcular el alfa hex a ojo es difícil; para alfa, `rgb()`/`hsl()` con `/ 0.5` suelen ser más legibles.

## Mayúsculas o minúsculas

Hex es **insensible a mayúsculas**: `#CBA6F7` y `#cba6f7` son idénticos. La convención más común es minúsculas. Lo importante es ser consistente en el proyecto.

## hex vs. rgb

> [!info] Mismo color, distinta sintaxis
> `#cba6f7` y `rgb(203 166 247)` son **exactamente** el mismo color en sRGB. La diferencia es de ergonomía:
> - **hex**: compacto, copiable de cualquier herramienta de diseño, ubicuo.
> - **rgb()**: más legible para ver/ajustar los canales, mejor para el alfa.
>
> Ninguno permite "aclarar" o "saturar" de forma intuitiva: para eso, [[04 HSL y HSLA | HSL]] u [[06 LAB y LCH | OKLCH]].

## Buenas prácticas

> [!tip] Recomendaciones
> - hex es una opción segura y universal para definir colores.
> - Usa la forma de 6 (u 8 con alfa); reserva la de 3 para colores "redondos".
> - Para opacidad legible, considera `rgb(... / 0.5)` en vez de calcular el alfa hex.
> - Guarda los colores hex en [[10 Variables CSS/index | variables CSS]], no repartidos por la hoja.

## Errores comunes

> [!warning] Trampas
> - **Olvidar el `#`**: `cba6f7` sin almohadilla no es un color válido.
> - **Confundir 3 vs. 6 dígitos**: `#f0a` ≠ `#f06000`; se duplica cada dígito.
> - **Alfa hex impreciso**: difícil de calcular a mano; `80` no es exactamente 50 %.

## Notas relacionadas

- [[03 RGB y RGBA]] — el mismo modelo con sintaxis numérica y alfa claro.
- [[01 Palabras Clave de Color]] — alternativa legible pero limitada.
- [[04 HSL y HSLA]] — para ajustar luz y saturación a mano.
