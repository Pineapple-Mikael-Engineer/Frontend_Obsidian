---
title: <meta charset> — Codificación de caracteres del documento
aliases:
  - charset
  - character encoding
tags:
  - html
  - api/elemento
  - metadatos
elemento: meta
categoria: metadatos
rol_implicito: ninguno
vacio: true
draft: false
---

# Codificación de Caracteres (meta charset)

> [!definicion]
> `<meta charset="UTF-8">` declara con qué **codificación** está guardado el archivo, para que el
> navegador traduzca correctamente cada secuencia de bytes a caracteres. Sin ella —o con la
> equivocada— las tildes, la `ñ` y los emojis pueden mostrarse como símbolos rotos (`Ã±`, `â€™`,
> `ðŸ˜€`), un fenómeno llamado *mojibake*.

```html
<head>
  <meta charset="UTF-8" />
  <!-- … -->
</head>
```

Es un elemento void (sin etiqueta de cierre) y debe ir **lo primero** dentro del
[[index | `<head>`]].

## El problema que resuelve: bytes vs. caracteres

Un archivo de texto es una secuencia de **bytes**; un carácter como `é` puede representarse de varias
maneras según la codificación. El navegador, al recibir los bytes, necesita saber qué tabla usar para
reconstruir las letras. Si asume una codificación distinta de la real, traduce mal:

| Carácter | Guardado como UTF-8 | Leído como ISO-8859-1 (mal) |
|----------|--------------------|-----------------------------|
| `é` | bytes `C3 A9` | `Ã©` |
| `ñ` | bytes `C3 B1` | `Ã±` |
| `'` (comilla tipográfica) | bytes `E2 80 99` | `â€™` |

Por eso `charset` no es opcional en cuanto el contenido sale del inglés básico ASCII.

## Por qué UTF-8 y por qué primero

### Siempre UTF-8

`UTF-8` es la codificación universal: cubre **todo** Unicode (cualquier idioma, símbolos, emojis) y
es el estándar de facto de la web (>98 % de los sitios). Las alternativas son legado y no deben
usarse en proyectos nuevos:

- `ISO-8859-1` (Latin-1) — solo Europa occidental, sin emojis ni la mayoría de símbolos.
- `Windows-1252` — variante de Microsoft, fuente clásica de las comillas rotas.

### Dentro de los primeros 1024 bytes

El parser empieza a leer **antes** de conocer la codificación. Si la declaración de `charset` aparece
tarde, el navegador ya pudo malinterpretar parte del documento y debe **reiniciar** el parseo desde
cero, perdiendo tiempo. La especificación exige que esté en los primeros 1024 bytes; en la práctica,
se pone como primera línea del `<head>`.

## Sintaxis moderna y heredada

```html
<!-- Forma moderna (HTML5) — usar esta -->
<meta charset="UTF-8" />

<!-- Forma heredada equivalente — no usar en proyectos nuevos -->
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
```

Ambas funcionan, pero la corta la reemplaza. No declarar las dos a la vez.

## Quién gana: archivo, etiqueta y servidor

Declarar `UTF-8` **no convierte** el archivo: solo informa de cómo interpretarlo. Hay tres fuentes de
verdad, y su prioridad es:

1. **Cabecera HTTP** `Content-Type: text/html; charset=…` del servidor (máxima prioridad).
2. **`<meta charset>`** del documento.
3. Heurística del navegador (si no hay nada, adivina; suele fallar).

Lo crítico es que las tres **coincidan** con cómo se guardó realmente el archivo en el editor.

## Buenas prácticas

> [!tip] Recomendaciones
> - **Guarda siempre los `.html` como UTF-8** en el editor (en VS Code, esquina inferior derecha;
>   preferentemente *sin* BOM).
> - Pon `<meta charset="UTF-8">` como **primera línea** del `<head>`, sin excepción.
> - Si ves *mojibake*, revisa en este orden: codificación de guardado del archivo → cabecera HTTP del
>   servidor → esta etiqueta.

## Errores comunes

> [!warning] Trampas
> - **Declarar UTF-8 pero guardar el archivo en otra codificación**: la etiqueta miente y los
>   caracteres se rompen igual. La etiqueta describe, no transforma.
> - **Ponerla tarde** (tras un bloque grande de comentarios o scripts): fuerza un reparseo.
> - **El BOM** (Byte Order Mark) al inicio del archivo puede causar problemas sutiles en algunos
>   servidores; guardar "UTF-8 sin BOM" lo evita.

## Notas relacionadas

- [[index]] — el `<head>` y por qué este `<meta>` va primero.
- [[02 Viewport (meta viewport)]] — el otro `<meta>` imprescindible, justo después.
