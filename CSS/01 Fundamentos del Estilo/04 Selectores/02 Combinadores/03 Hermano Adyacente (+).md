---
title: Combinador Hermano Adyacente (+) — El hermano inmediato siguiente
aliases:
  - adjacent sibling combinator
tags:
  - css
  - api/selector
  - arquitectura
selector: hermano-adyacente
draft: false
---

# Hermano Adyacente (+)

> [!definicion]
> El combinador de **hermano adyacente** (`+`) selecciona el elemento que viene **inmediatamente después** de otro, siendo ambos **hermanos** (mismo padre). `A + B` significa "el `B` que sigue justo a un `A`".

```css
h2 + p { margin-top: 0; }
/* el primer <p> que va inmediatamente tras un <h2> */
```

```html
<h2>Título</h2>
<p>Afectado</p>      <!-- ✅ inmediatamente después -->
<p>No afectado</p>   <!-- ❌ no es el inmediato -->
```

## Casos de uso típicos

| Patrón | Selector |
|--------|----------|
| Quitar margen al primer párrafo tras un título | `h2 + p` |
| Espaciar elementos consecutivos (lobotomized owl) | `* + *` |
| Estilar la etiqueta tras un checkbox marcado | `input:checked + label` |
| Separador entre ítems | `.item + .item` |

## El patrón "lobotomized owl"

> [!tip] * + * para espaciado entre hermanos
> Un patrón famoso para espaciar elementos sin margen en los extremos: aplicar margen superior solo a los elementos que **siguen** a otro:
> ```css
> .flujo > * + * { margin-top: 1rem; }
> ```
> Así el primer hijo no lleva margen (no sigue a nadie) y el resto se separan uniformemente. Elegante y sin "primer/último hijo".

## El truco del checkbox

`input:checked + label` (o un hermano) permite reaccionar al estado de un control sin JavaScript, base de patrones como menús o acordeones "solo CSS":

```css
#toggle:checked + .menu { display: block; }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Úsalo para relaciones "justo después de" (título → primer párrafo).
> - `* + *` es un patrón limpio de espaciado entre hermanos.
> - Combínalo con `:checked` para interacciones sin JS.
> - Recuerda: solo el hermano **inmediato**; para todos los siguientes, usa `~`.

## Errores comunes

> [!warning] Trampas
> - **Esperar que afecte a todos los hermanos siguientes**: solo al inmediato (eso es `~`).
> - **Elementos no hermanos**: deben compartir padre.

## Notas relacionadas

- [[04 Hermano General (~)]] — todos los hermanos siguientes, no solo el inmediato.
- [[02 Hijo Directo (>)]] — relación de anidación, no de hermandad.
- [[01 checked e indeterminate]] — el truco del checkbox.
