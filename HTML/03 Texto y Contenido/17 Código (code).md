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
> `<code>` marca un fragmento de **código fuente** o el nombre de algo del lenguaje de programación: una función, una variable, una etiqueta, un comando. El navegador lo muestra en tipografía **monoespaciada** por defecto.

```html
<p>La función <code>map()</code> transforma cada elemento del array.</p>
```

## code en línea vs. en bloque

| Forma | Marcado | Uso |
|-------|---------|-----|
| En línea | `<code>` solo | Un identificador o expresión dentro de una frase |
| En bloque | `<pre><code>` | Un fragmento de varias líneas con formato preservado |

Para varias líneas de código se anida dentro de [[18 Código Preformateado (pre) | `<pre>`]], que conserva espacios y saltos. `<code>` por sí solo no preserva el formato (el HTML colapsa los espacios).

## Escapar los caracteres especiales

> [!warning] <, > y & deben escaparse
> Dentro de `<code>`, los caracteres `<`, `>` y `&` se interpretan como marcado HTML. Para mostrarlos literalmente hay que escribirlos como **entidades**:
> | Carácter | Entidad |
> |----------|---------|
> | `<` | `&lt;` |
> | `>` | `&gt;` |
> | `&` | `&amp;` |
>
> Para mostrar `<div>` en pantalla hay que escribir `<code>&lt;div&gt;</code>`. Olvidarlo hace que el navegador intente interpretar `<div>` como un elemento real.

## El coloreado de sintaxis no es de code

> [!info] Quién pinta los colores
> `<code>` solo aporta la tipografía monoespaciada y el significado semántico "esto es código". El **resaltado de sintaxis** (colores por palabra clave, cadena, número) lo añade una librería de JavaScript (Prism, highlight.js, Shiki) o el propio entorno —como este vault de Obsidian—, no el HTML por sí mismo. Un `<code>` sin librería se ve monoespaciado pero monocromo.

## Ejemplos en contexto

```html
<p>Usa <code>const</code> para constantes y <code>let</code> para variables.</p>
<p>El atributo <code>srcset</code> permite imágenes responsivas.</p>
<p>Ejecuta <code>npm install</code> en la terminal.</p>
```

## Buenas prácticas

> [!tip] Recomendaciones
> - `<code>` en línea para identificadores y expresiones cortas dentro de prosa.
> - `<pre><code>` para bloques multilínea.
> - Escapa siempre `<`, `>`, `&` dentro de código.
> - Para nombres de variables conceptuales o marcadores de posición, considera [[19 Variable (var) | `<var>`]].

## Errores comunes

> [!warning] Trampas
> - **No escapar** los caracteres especiales: el navegador interpreta el código como HTML.
> - **Usar `<code>` para texto monoespaciado decorativo** que no es código: el monoespaciado estético es CSS (`font-family: monospace`).
> - **Esperar colores sin librería**: el resaltado de sintaxis requiere JS o el entorno.

## Notas relacionadas

- [[18 Código Preformateado (pre)]] — para bloques de código multilínea.
- [[19 Variable (var)]] — variables y marcadores de posición.
- [[20 Salida de Programa (samp)]] — la salida de un programa.
- [[21 Entrada de Teclado (kbd)]] — la entrada del usuario por teclado.
