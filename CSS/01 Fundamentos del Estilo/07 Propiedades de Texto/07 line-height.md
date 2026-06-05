---
title: line-height — Altura de línea (interlineado)
aliases:
  - line-height
  - interlineado
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: line-height
grupo: tipografia
valor_inicial: normal
hereda: true
animable: true
draft: false
---

# line-height

> [!definicion]
> `line-height` define la **altura de cada línea** de texto: el espacio vertical que ocupa, lo que determina el **interlineado**. Es una de las propiedades con más impacto en la **legibilidad**: un interlineado adecuado hace el texto cómodo de leer; uno apretado o excesivo, fatigoso.

```css
body { line-height: 1.6; }    /* recomendado para texto corrido */
h1   { line-height: 1.1; }    /* títulos: más ajustado */
```

## Tipos de valor

| Valor | Ejemplo | Comportamiento |
|-------|---------|----------------|
| **Número (sin unidad)** | `1.6` | Multiplica el `font-size` del **propio** elemento (recomendado) |
| Longitud | `24px`, `1.5rem` | Altura fija |
| Porcentaje | `150%` | Respecto al `font-size` del elemento |
| `normal` | — | El navegador elige (~1.2, depende de la fuente) |

## El número sin unidad es el correcto

> [!tip] Usa line-height sin unidad
> La forma recomendada es un **número sin unidad** (`1.6`), no una longitud. La diferencia es crucial al heredar:
> - **`line-height: 1.6`** (número): cada elemento descendiente multiplica `1.6` por **su propio** `font-size`. Un título grande tendrá interlineado proporcional.
> - **`line-height: 24px`** (longitud): el valor **calculado** (24px) se hereda fijo, así que un título de 40px hereda un interlineado de solo 24px → las líneas se solapan.
>
> ```css
> body { line-height: 1.6; }   /* ✅ cada elemento escala su interlineado */
> body { line-height: 24px; }  /* ❌ se hereda fijo, rompe títulos grandes */
> ```

## Valores recomendados

| Contexto | line-height |
|----------|-------------|
| Texto corrido (párrafos) | 1.5–1.7 |
| Títulos grandes | 1.1–1.3 |
| Texto muy pequeño | Un poco más (mejora legibilidad) |
| Interfaz compacta | ~1.4 |

Las WCAG recomiendan al menos **1.5** para el cuerpo de texto por accesibilidad (criterio de espaciado de texto).

## Cómo se distribuye el espacio

> [!info] El leading se reparte arriba y abajo
> La diferencia entre el `line-height` y la altura real de las letras (el *leading*) se reparte **mitad arriba, mitad abajo** de cada línea. Por eso un `line-height` mayor separa las líneas simétricamente. La primera línea también recibe su mitad superior, lo que añade un pequeño espacio sobre el texto.

## line-height y la altura de los elementos en línea

`line-height` también afecta a la altura de cajas en línea y es una vieja técnica para **centrar verticalmente** una sola línea de texto (un botón, una etiqueta): poner `line-height` igual a la altura del contenedor. Con Flexbox (`align-items: center`) hoy es más robusto, pero el truco persiste para casos simples.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa un **número sin unidad** (`1.5`–`1.7` para cuerpo) para que el interlineado escale al heredar.
> - Títulos más ajustados (1.1–1.3); cuerpo más holgado.
> - Mínimo 1.5 en texto corrido por accesibilidad.
> - Para centrar una línea, considera Flexbox antes que el truco de `line-height`.

## Errores comunes

> [!warning] Trampas
> - **`line-height` con unidad** heredado: rompe el interlineado de elementos de otro tamaño.
> - **Interlineado apretado** (< 1.4) en cuerpo: cansa la lectura.
> - **`normal`** sin pensar: varía entre fuentes y suele quedar corto para cuerpo.

## Notas relacionadas

- [[03 font-size]] — la referencia que el número de `line-height` multiplica.
- [[01 Herencia Natural]] — por qué el número sin unidad hereda mejor.
- [[01 Contenedor Flex (display flex)]] — centrar verticalmente sin el truco de line-height.
