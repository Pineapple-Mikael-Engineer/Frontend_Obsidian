---
title: <br> — Salto de línea
aliases:
  - line break
  - br
tags:
  - html
  - api/elemento
  - semantica
elemento: br
categoria: fraseo
rol_implicito: ninguno
vacio: true
draft: false
order: 2
---

# Saltos de Línea (br)

> [!definicion]
> `<br>` fuerza un **salto de línea** dentro de un bloque de texto, sin iniciar un párrafo nuevo. Es
> un elemento void (sin cierre). Solo es semánticamente correcto cuando el salto **forma parte del
> contenido**: poesía, direcciones postales, letras de canciones, una firma.

```html
<p>
  Calle Mayor 12<br />
  28013 Madrid<br />
  España
</p>
```

## Por qué el HTML colapsa los saltos normales

En el texto HTML, todos los espacios, tabulaciones y saltos de línea consecutivos del código fuente
se **colapsan en un solo espacio** al renderizar. Por eso no basta pulsar Enter en el editor para
saltar de línea en la página: hace falta un `<br>` explícito (o un nuevo bloque, o CSS
`white-space: pre`).

## Usos correctos vs. incorrectos

| Correcto (el salto es contenido) | Incorrecto (el salto es maquetación) |
|----------------------------------|--------------------------------------|
| Versos de un poema | Separar dos párrafos → usar `<p>` |
| Líneas de una dirección postal | Crear espacio vertical → usar CSS `margin` |
| Firma en varias líneas | Maquetar columnas → usar CSS layout |
| Letra de una canción | Forzar que algo "baje" → usar CSS |

```html
<!-- ✅ El salto pertenece al poema -->
<p>
  Verde que te quiero verde.<br />
  Verde viento. Verdes ramas.
</p>

<!-- ❌ br para separar bloques: usa <p> -->
<p>Primer tema.<br /><br />Segundo tema.</p>
```

## Accesibilidad

> [!info] Cómo lo interpreta la asistencia
> Un `<br>` se anuncia como una pausa breve en la lectura. Abusar de ellos para maquetar produce
> lecturas entrecortadas y confusas. Una dirección con `<br>` se lee de corrido con pausas naturales;
> diez `<br>` para "bajar" un elemento generan ruido. La separación entre bloques se hace con `<p>` y
> márgenes, que la asistencia entiende como estructura.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<br>` solo cuando el salto **es** parte del texto.
> - Para separar párrafos: [[01 Párrafos (p) | `<p>`]]. Para espacio: `margin` en CSS.
> - Para un cambio de tema: [[03 Línea Horizontal (hr) | `<hr>`]].

## Errores comunes

> [!warning] br como herramienta de espaciado
> El antipatrón más extendido es encadenar `<br><br>` para separar contenido. Rompe la semántica y es
> frágil ante cambios de diseño. Toda separación visual entre bloques es responsabilidad del CSS, no
> de saltos de línea repetidos.

## Notas relacionadas

- [[01 Párrafos (p)]] — para separar bloques de texto, en lugar de `<br>`.
- [[25 Ruptura de Palabra (wbr)]] — un punto de salto **opcional**, no forzado.
- [[03 Línea Horizontal (hr)]] — separación temática, no solo de línea.
