---
title: font-optical-sizing — Ajuste óptico según el tamaño
aliases:
  - font-optical-sizing
  - optical sizing
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: font-optical-sizing
grupo: tipografia
valor_inicial: auto
hereda: true
animable: false
draft: false
---

# font-optical-sizing

> [!definicion]
> `font-optical-sizing` activa el **ajuste óptico**: en fuentes variables con eje `opsz`, las letras ajustan sutilmente su diseño según el **tamaño** al que se muestran, para verse mejor tanto en texto pequeño como en titulares grandes.

```css
body { font-optical-sizing: auto; }   /* ajuste automático (por defecto) */
```

## Valores

| Valor | Efecto |
|-------|--------|
| `auto` | El navegador ajusta el diseño según el `font-size` (por defecto) |
| `none` | Desactiva el ajuste óptico |

## Qué es el tamaño óptico

> [!info] Las fuentes se diseñaban distintas según el tamaño
> En la tipografía tradicional (metal), una fuente para texto pequeño (cuerpo) se diseñaba **distinta** que para titulares grandes: en tamaños pequeños, las letras necesitan **más grosor, más espacio y rasgos más robustos** para ser legibles; en grandes, pueden ser más finas y elegantes. El eje **`opsz`** de una fuente variable recupera esto: ajusta automáticamente esos rasgos según el tamaño de uso.

```
Texto pequeño (opsz bajo):  rasgos más robustos, más espaciado
Titular grande (opsz alto): rasgos más finos y refinados
```

## Funciona solo con fuentes que tengan eje opsz

> [!warning] Requiere una fuente variable con opsz
> `font-optical-sizing` solo tiene efecto si la fuente es **variable** y define el eje **`opsz`**. Fuentes como Inter, Roboto Flex o las de Apple lo incluyen; muchas no. En fuentes sin ese eje, la propiedad no hace nada (pero tampoco rompe).

## auto suele ser lo correcto

Con `auto` (el valor por defecto), el navegador vincula el `opsz` al `font-size` automáticamente: el texto pequeño usa el diseño robusto y los titulares el refinado, sin que tengas que hacer nada. Solo se desactiva (`none`) en casos muy concretos donde el ajuste no se desea.

## Buenas prácticas

> [!tip] Recomendaciones
> - Deja `auto` (por defecto): el ajuste mejora la legibilidad sin esfuerzo.
> - Usa fuentes variables con eje `opsz` para aprovecharlo (Inter, Roboto Flex…).
> - `none` solo si necesitas un diseño uniforme entre tamaños por algún motivo.
> - Para forzar un valor concreto del eje, usa [[03 font-variation-settings | `"opsz"`]].

## Errores comunes

> [!warning] Trampas
> - **Esperar efecto** en una fuente sin eje `opsz`: no hace nada.
> - **Confundirlo con `font-size`**: `opsz` ajusta el **diseño**, no el tamaño.

## Notas relacionadas

- [[03 font-variation-settings]] — controlar `opsz` manualmente.
- [[03 font-size]] — el tamaño al que se vincula el ajuste óptico.
- [[01 Fuentes Web (@font-face)/index]] — cargar una fuente variable con `opsz`.
