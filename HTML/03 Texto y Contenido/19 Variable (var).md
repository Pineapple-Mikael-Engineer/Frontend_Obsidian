---
title: <var> — Variable matemática o de programación
aliases:
  - var
  - variable
tags:
  - html
  - api/elemento
  - semantica
elemento: var
categoria: fraseo
rol_implicito: ninguno
vacio: false
draft: false
---

# Variable (var)

> [!definicion]
> `<var>` marca una **variable**: el nombre de una variable en una expresión matemática o de programación, o un marcador de posición que el usuario debe sustituir por un valor real. Se rinde en cursiva por defecto.

```html
<p>El área del rectángulo es <var>base</var> × <var>altura</var>.</p>
<p>Ejecuta <code>git clone <var>url-del-repo</var></code>.</p>
```

## El grupo de elementos técnicos

`<var>` forma parte de una familia que conviene ver junta, porque se reparten los distintos roles del texto técnico:

| Elemento | Representa | Ejemplo |
|----------|------------|---------|
| `<code>` | Código fuente literal | `<code>map()</code>` |
| `<var>` | Variable o marcador de posición | `<var>x</var>`, `<var>nombre</var>` |
| `<samp>` | Salida de un programa | `<samp>Error 404</samp>` |
| `<kbd>` | Entrada del usuario por teclado | `<kbd>Ctrl</kbd>+<kbd>C</kbd>` |

El detalle de cada uno: [[17 Código (code) | code]], [[20 Salida de Programa (samp) | samp]] y [[21 Entrada de Teclado (kbd) | kbd]].

## Combinar var con code

El uso más útil es distinguir, dentro de un comando, la parte **fija** (literal) de la parte **sustituible** (variable):

```html
<p>Para clonar: <code>git clone <var>url</var></code></p>
<p>Para moverte: <code>cd <var>directorio</var></code></p>
<p>Fórmula: <var>v</var> = <var>d</var> / <var>t</var></p>
```

El lector entiende de inmediato qué debe reemplazar (`url`, `directorio`) y qué se escribe tal cual (`git clone`, `cd`).

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<var>` para variables matemáticas, de programación y marcadores de posición.
> - Combínalo con `<code>` para separar lo literal de lo sustituible en comandos.
> - Para fórmulas complejas con notación matemática avanzada, considera MathML o una librería como KaTeX; `<var>` cubre la notación sencilla en línea.

## Errores comunes

> [!warning] var como cursiva genérica
> La cursiva de `<var>` es semántica (es una variable), no estética. Para nombres científicos o títulos va [[07 Cursiva sin Énfasis (i) | `<i>`]]; para énfasis de tono, [[05 Énfasis (em) | `<em>`]]; para cursiva puramente decorativa, CSS. Reservar `<var>` para variables reales mantiene el significado.

## Notas relacionadas

- [[17 Código (code)]] — lo literal frente a la variable sustituible.
- [[20 Salida de Programa (samp)]] — la salida de un programa.
- [[21 Entrada de Teclado (kbd)]] — la entrada por teclado.
