---
title: text-orientation — Orientación de los glifos en vertical
aliases:
  - text-orientation
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: text-orientation
grupo: tipografia
valor_inicial: mixed
hereda: true
animable: false
draft: false
---

# text-orientation

> [!definicion]
> `text-orientation` decide cómo se **orientan los caracteres** cuando el texto está en modo vertical ([[01 writing-mode | `writing-mode: vertical-*`]]): si cada glifo se mantiene en pie (apilado, para CJK) o se gira de lado (para texto latino). Solo tiene efecto en escritura vertical.

```css
.vertical {
  writing-mode: vertical-rl;
  text-orientation: mixed;   /* CJK en pie, latino girado */
}
```

## Valores

| Valor | Glifos CJK | Glifos latinos/números |
|-------|-----------|------------------------|
| `mixed` | En pie (vertical) | Girados 90° (de lado) — por defecto |
| `upright` | En pie | **En pie** (apilados uno sobre otro) |
| `sideways` | Girados 90° | Girados 90° (todo de lado) |

## El comportamiento por defecto: mixed

> [!info] mixed: cada sistema en su orientación natural
> `mixed` (por defecto) es lo más sensato para texto japonés/chino mezclado con latín: los **ideogramas CJK** se mantienen en pie (su orientación natural en vertical), mientras que el **texto latino y los números** se giran 90° (de lado), porque apilarlos uno sobre otro sería ilegible. Así "CSS 2024" dentro de un texto vertical japonés aparece de lado, legible girando la cabeza.

## upright: latino apilado

`upright` fuerza **todos** los caracteres en pie, incluidos los latinos, que quedan apilados uno encima de otro. Es legible para siglas cortas pero ilegible para palabras largas. Su uso es de nicho (algún efecto editorial):

```
upright:  C
          S
          S
```

## sideways: todo girado

`sideways` gira **todo** el texto 90°, como si pusieras una línea horizontal de lado. Útil para etiquetas latinas verticales, aunque [[01 writing-mode | `writing-mode: sideways-lr`]] suele ser más directo para eso.

## Buenas prácticas

> [!tip] Recomendaciones
> - Deja `mixed` (por defecto) para texto vertical CJK con latín intercalado.
> - `upright` solo para siglas/caracteres sueltos que deban ir en pie.
> - Para texto **latino** vertical, considera `sideways-lr` en `writing-mode` en vez de `text-orientation`.
> - Recuerda que solo funciona con `writing-mode` vertical.

## Errores comunes

> [!warning] Trampas
> - **Esperar efecto** sin `writing-mode: vertical-*`: no hace nada en horizontal.
> - **`upright` con palabras largas** latinas: ilegibles apiladas.
> - **Confundir orientación de glifo con dirección de línea** (eso es `writing-mode`/`direction`).

## Notas relacionadas

- [[01 writing-mode]] — el modo vertical que `text-orientation` complementa.
- [[02 text-emphasis]] — marcas de énfasis CJK, mismo contexto.
- [[02 direction]] — la dirección del texto en línea.
