---
title: <code> — Fragmento de código en línea
aliases:
  - code
  - inline code
tags:
  - html
  - api/elemento
  - semantica
elemento: code
categoria: fraseo
rol_implicito: ninguno
vacio: false
draft: false
---

# Código (code)

> [!definicion]
> `<code>` marca un fragmento de **código fuente** o el nombre de algo del lenguaje de programación
> (una función, una variable, una etiqueta). El navegador lo muestra en tipografía **monoespaciada**
> por defecto.

```html
<p>La función <code>map()</code> transforma cada elemento del array.</p>
```

## code en línea vs. en bloque

| Forma | Marcado | Uso |
|-------|---------|-----|
| En línea | `<code>` solo | Un identificador dentro de una frase |
| En bloque | `<pre><code>` | Un fragmento de varias líneas con formato preservado |

Para varias líneas de código se anida dentro de [[18 Código Preformateado (pre) | `<pre>`]], que
conserva espacios y saltos.

> [!warning] Escapar los caracteres especiales
> Dentro de `<code>`, los caracteres `<`, `>` y `&` deben escribirse como entidades (`&lt;`, `&gt;`,
> `&amp;`); de lo contrario el navegador los interpreta como marcado. Para mostrar `<div>` hay que
> escribir `&lt;div&gt;`.

> [!info] No aplica resaltado de sintaxis
> `<code>` solo da la tipografía monoespaciada y el significado semántico. El **coloreado de
> sintaxis** lo añade una librería de JavaScript (Prism, highlight.js) o el entorno (como este
> vault), no el HTML por sí solo.

## Notas relacionadas

- [[18 Código Preformateado (pre)]] — para bloques de código multilínea.
- [[19 Variable (var)]], [[20 Salida de Programa (samp)]], [[21 Entrada de Teclado (kbd)]] — el resto de elementos técnicos.
