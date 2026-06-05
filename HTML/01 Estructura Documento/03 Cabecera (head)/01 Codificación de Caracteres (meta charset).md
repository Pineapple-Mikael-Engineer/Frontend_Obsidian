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
> navegador interprete correctamente cada byte como carácter. Sin ella, las tildes, la `ñ` y los
> emojis pueden mostrarse como símbolos rotos (`Ã±`, `â€™`).

```html
<head>
  <meta charset="UTF-8" />
  <!-- … -->
</head>
```

Es un elemento [void](#) (sin etiqueta de cierre) y debe ir **lo primero** dentro del
[[index | `<head>`]].

## Por qué UTF-8 y por qué primero

`UTF-8` es la codificación universal: cubre todos los idiomas y símbolos Unicode, y es el estándar
de facto de la web. **Siempre `UTF-8`**; las alternativas (`ISO-8859-1`, `Windows-1252`) son legado.

El parser lee bytes antes de saber la codificación. Si el `charset` aparece tarde, ya pudo haber
malinterpretado parte del documento y debe **reiniciar** el parseo. Por eso la regla: dentro de los
**primeros 1024 bytes** del documento.

> [!info] Sintaxis heredada
> La forma antigua `<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">` sigue
> siendo válida, pero `<meta charset="UTF-8">` la reemplaza en HTML5 por ser más corta. No usar
> ambas.

> [!warning] El charset del archivo y el declarado deben coincidir
> Declarar `UTF-8` no convierte el archivo: solo informa. Si el editor guarda el `.html` en otra
> codificación, los caracteres se rompen igual. Guardar siempre el archivo como UTF-8 (sin BOM
> preferentemente). La cabecera HTTP `Content-Type` del servidor, si existe, tiene prioridad sobre
> esta etiqueta.

## Notas relacionadas

- [[index]] — el `<head>` donde reside y por qué va primero.
- [[02 Viewport (meta viewport)]] — el otro `<meta>` imprescindible.
