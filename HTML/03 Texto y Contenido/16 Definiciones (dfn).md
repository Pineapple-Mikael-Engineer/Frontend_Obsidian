---
title: <dfn> — Término que se está definiendo
aliases:
  - dfn
  - defining instance
tags:
  - html
  - api/elemento
  - semantica
elemento: dfn
categoria: fraseo
rol_implicito: ninguno
vacio: false
draft: false
---

# Definiciones (dfn)

> [!definicion]
> `<dfn>` marca el **término que se define** en el párrafo actual: la instancia donde ese concepto
> queda definido por primera vez. El navegador lo muestra en cursiva por defecto.

```html
<p>Una <dfn>API</dfn> es un conjunto de reglas que permite a dos
   programas comunicarse entre sí.</p>
```

El `<dfn>` envuelve el **término**, no su definición: la definición es el resto de la frase.

## Asociar término y significado

| Patrón | Efecto |
|--------|--------|
| `<dfn>API</dfn>` | El término es el contenido del elemento |
| `<dfn title="...">...</dfn>` | El `title` da la forma exacta del término definido |
| `<dfn><abbr title="...">API</abbr></dfn>` | Combina acrónimo y definición |

> [!info] Un término, una definición
> La convención es que cada término tenga **una** instancia `<dfn>` en el documento (donde se
> define), y que el resto de apariciones sean texto normal o enlaces a esa definición.

> [!tip] Combinar con abbr
> Para un acrónimo que se define, anidar [[15 Abreviaturas (abbr) | `<abbr>`]] dentro de `<dfn>`: se
> marca a la vez como término definido y como abreviatura con su forma completa.

## Notas relacionadas

- [[15 Abreviaturas (abbr)]] — se anida dentro para acrónimos.
- [[04 Listas de Definición (dl, dt, dd)]] — listas de términos y definiciones.
