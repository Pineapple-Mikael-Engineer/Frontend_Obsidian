---
title: text-decoration — Subrayado, tachado y líneas
aliases:
  - text-decoration
  - underline
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: text-decoration
grupo: tipografia
valor_inicial: none
hereda: false
animable: false
draft: false
order: 9
---

# text-decoration

> [!definicion]
> `text-decoration` añade **líneas** al texto: subrayado, línea superior, tachado. Es un shorthand de varias propiedades que controlan la línea, su estilo, color y grosor. Su uso más visible es el subrayado de los enlaces.

```css
a            { text-decoration: underline; }
.tachado     { text-decoration: line-through; }
a.sin-raya   { text-decoration: none; }
```

## El shorthand y sus partes

```css
text-decoration: underline wavy red 2px;
/*               line       style color thickness */
```

| Sub-propiedad | Valores | Controla |
|---------------|---------|----------|
| `text-decoration-line` | `none`, `underline`, `overline`, `line-through` | Qué línea(s) |
| `text-decoration-style` | `solid`, `double`, `dotted`, `dashed`, `wavy` | El estilo de la línea |
| `text-decoration-color` | un color | El color de la línea |
| `text-decoration-thickness` | longitud o `auto` | El grosor |

## El subrayado de los enlaces

> [!warning] No quites el subrayado de los enlaces sin alternativa
> Quitar el subrayado de los enlaces (`text-decoration: none`) sin otra señal visual los hace **indistinguibles** del texto normal, una barrera de accesibilidad: el color por sí solo no basta (usuarios con daltonismo no lo perciben). Si lo quitas, da otra pista (negrita, otro color **con** suficiente contraste, subrayado en `:hover`). El subrayado es la convención universal de "enlace".

## Afinar el subrayado

> [!tip] underline-offset y skip-ink
> Dos propiedades relacionadas mejoran mucho el subrayado:
> - [[04 text-underline-offset y position | `text-underline-offset`]]: separa la línea del texto para que no toque las letras.
> - `text-decoration-skip-ink: auto` (por defecto): la línea **salta** las partes descendentes de las letras (la "g", la "p"), quedando más limpia.
> ```css
> a { text-decoration-thickness: 2px; text-underline-offset: 3px; }
> ```

## Herencia particular

> [!info] Se "propaga" pero no se hereda como tal
> `text-decoration` no se hereda en el sentido normal, pero la línea **se extiende** a los descendientes en línea (un `<strong>` dentro de un enlace subrayado también queda subrayado), porque la decoración la "pinta" el elemento que la declara sobre todo su texto. Quitarla en un hijo con `text-decoration: none` no siempre funciona como se espera por esta razón.

## Buenas prácticas

> [!tip] Recomendaciones
> - Mantén (o repón) una señal visual en los enlaces; el subrayado es la convención.
> - Afina con `text-underline-offset` y `thickness` para un subrayado elegante.
> - Usa `line-through` para precios tachados, pero recuerda que la asistencia no lo anuncia (ver [[10 Tachado (s, del) | `<s>`/`<del>`]]).
> - Para subrayados decorativos animados, considera `background` con gradiente en vez de `text-decoration`.

## Errores comunes

> [!warning] Trampas
> - **Enlaces sin subrayado** ni otra pista: inaccesibles.
> - **Subrayar texto que no es enlace**: confunde (el usuario intenta hacer clic).
> - **Esperar quitar la decoración en un hijo**: la línea del ancestro puede persistir.

## Notas relacionadas

- [[04 text-underline-offset y position]] — afinar la posición del subrayado.
- [[09 Subrayado (u)]] — el subrayado desde HTML y su problema.
- [[07 visited]] — estilar enlaces visitados.
