---
title: <pre> — Texto preformateado
aliases:
  - pre
  - preformatted text
tags:
  - html
  - api/elemento
  - semantica
elemento: pre
categoria: flujo
rol_implicito: ninguno
vacio: false
draft: false
---

# Código Preformateado (pre)

> [!definicion]
> `<pre>` muestra el texto **tal cual está escrito**: conserva espacios, tabulaciones y saltos de
> línea, en lugar de colapsarlos como hace el HTML normal. Se rinde en tipografía monoespaciada. Es
> el contenedor estándar de bloques de código.

```html
<pre><code>function saludar(nombre) {
  return `Hola, ${nombre}`;
}</code></pre>
```

## Por qué pre + code juntos

| Elemento | Aporta |
|----------|--------|
| `<pre>` | Preserva el **formato** (espacios y saltos) |
| [[17 Código (code) | `<code>`]] | Aporta el **significado** (esto es código) |

`<pre>` solo conserva el formato; `<code>` solo da semántica. Para un bloque de código se combinan:
`<pre><code>…</code></pre>`.

> [!warning] La indentación del HTML se ve
> Como `<pre>` respeta los espacios, la sangría del propio archivo HTML aparece en pantalla. Si el
> `<pre>` está indentado dentro del marcado, ese espacio extra se muestra. Por eso el contenido suele
> escribirse pegado al margen izquierdo.

> [!warning] Escapar `<`, `>`, `&`
> Igual que en `<code>`, los caracteres especiales deben escaparse (`&lt;`, `&gt;`, `&amp;`) o el
> navegador los interpreta como etiquetas.

> [!info] Otros usos
> Más allá del código, `<pre>` sirve para arte ASCII, salida de terminal o cualquier texto donde el
> espaciado **es** el contenido.

## Notas relacionadas

- [[17 Código (code)]] — se anida dentro para el significado semántico.
- [[20 Salida de Programa (samp)]] — para representar salida de programa en línea.
