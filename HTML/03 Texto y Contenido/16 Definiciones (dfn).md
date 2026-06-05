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
> `<dfn>` marca el **término que se define** en el párrafo actual: la instancia donde ese concepto queda definido por primera vez. El navegador lo muestra en cursiva por defecto. Envuelve el **término**, no su definición.

```html
<p>Una <dfn>API</dfn> es un conjunto de reglas que permite a dos programas
   comunicarse entre sí.</p>
```

El término es `<dfn>API</dfn>`; la definición ("es un conjunto de reglas…") es el resto de la frase, fuera del elemento.

## Cómo asociar término y significado

| Patrón | Qué define como término |
|--------|-------------------------|
| `<dfn>API</dfn>` | El propio contenido del elemento |
| `<dfn title="HyperText Markup Language">HTML</dfn>` | El `title` da la forma canónica del término |
| `<dfn><abbr title="…">API</abbr></dfn>` | Combina acrónimo (con su forma larga) y término definido |

Cuando el `title` está presente en un `<dfn>`, debe contener exactamente el término que se define.

## Una definición por término

> [!info] El convenio de unicidad
> La convención es que cada término tenga **una sola** instancia `<dfn>` en el documento —donde se define— y que las demás apariciones sean texto normal o enlaces a esa definición. El navegador puede asociar las menciones del término con su `<dfn>`: si en el mismo documento un `<abbr>`/texto coincide con el término definido, las herramientas pueden vincularlos.

## Ejemplo en un glosario

```html
<p><dfn id="def-dom">DOM</dfn>: representación en árbol del documento que el
   navegador construye y que JavaScript puede manipular.</p>
…
<p>Para cambiar el contenido, se modifica el <a href="#def-dom">DOM</a>.</p>
```

El `id` en el `<dfn>` permite enlazar a la definición desde cualquier mención posterior.

## Buenas prácticas

> [!tip] Recomendaciones
> - Marca con `<dfn>` la **primera** aparición, donde el término se define.
> - Dale un `id` si quieres enlazar a la definición desde otras menciones.
> - Para acrónimos, combina con `<abbr title>` para dar la forma completa.

## Errores comunes

> [!warning] Trampas
> - **Envolver la definición** en vez del término: `<dfn>` va sobre la palabra definida, no sobre la explicación.
> - **Marcar todas las apariciones** con `<dfn>`: solo la instancia que define; el resto es texto o enlace.
> - **Confundir cursiva semántica con estética**: la cursiva de `<dfn>` indica "término definido", no es decoración (eso sería `<i>` o CSS).

## Notas relacionadas

- [[15 Abreviaturas (abbr)]] — se anida dentro de `<dfn>` para acrónimos.
- [[04 Listas de Definición (dl, dt, dd)]] — listas de términos y definiciones.
