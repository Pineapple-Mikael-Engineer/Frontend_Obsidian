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
---

# Saltos de Línea (br)

> [!definicion]
> `<br>` fuerza un **salto de línea** dentro de un bloque de texto, sin iniciar un párrafo nuevo. Es
> un elemento void (sin cierre). Solo es semánticamente correcto cuando el salto **forma parte del
> contenido**: poesía, direcciones, letras de canciones.

```html
<p>
  Calle Mayor 12<br />
  28013 Madrid<br />
  España
</p>
```

## Usos correctos vs. incorrectos

| Correcto | Incorrecto |
|----------|------------|
| Versos de un poema | Separar dos párrafos (usar `<p>`) |
| Líneas de una dirección postal | Crear espacio vertical (usar CSS `margin`) |
| Firma en varias líneas | Maquetar una columna |

> [!warning] No para espaciar
> Encadenar `<br><br>` para separar bloques es un antipatrón clásico. La separación entre párrafos
> se logra con [[01 Párrafos (p) | `<p>`]] y márgenes CSS. `<br>` solo cuando el salto **pertenece al
> texto**.

> [!info] Accesibilidad
> Un `<br>` se anuncia como una pausa breve. Abusar de ellos para maquetar genera lecturas
> entrecortadas y confusas en lectores de pantalla. Su contraparte semántica fuerte es
> [[03 Línea Horizontal (hr) | `<hr>`]], que sí marca un cambio temático.

## Notas relacionadas

- [[01 Párrafos (p)]] — para separar bloques de texto, no `<br>`.
- [[25 Ruptura de Palabra (wbr)]] — punto de salto **opcional**, no forzado.
