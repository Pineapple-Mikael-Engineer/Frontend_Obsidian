---
title: <embed> — Incrustación de recurso externo
aliases:
  - embed element
tags:
  - html
  - api/elemento
  - multimedia
elemento: embed
categoria: incrustado
rol_implicito: ninguno
vacio: true
draft: false
---

# Incrustación Genérica (embed)

> [!definicion]
> `<embed>` incrusta un **recurso externo** (un PDF, contenido de una aplicación) de forma simple. Es un elemento void, sin contenido de respaldo. Es el más antiguo y limitado de los tres elementos de incrustación, y hoy de uso marginal.

```html
<embed src="documento.pdf" type="application/pdf" width="600" height="800" />
```

## Atributos

| Atributo | Descripción |
|----------|-------------|
| `src` | URL del recurso |
| `type` | Tipo MIME |
| `width` / `height` | Dimensiones |

## embed vs. object vs. iframe

| | `<embed>` | `<object>` | `<iframe>` |
|--|-----------|-----------|------------|
| Void (sin cierre) | Sí | No | No |
| Contenido de respaldo | No | Sí | No |
| `sandbox` | No | No | Sí |
| Recomendado hoy | Rara vez | PDF con respaldo | Páginas/servicios |

> [!info] El menos recomendado
> `<embed>` carece de las ventajas de los otros dos: sin respaldo (como `<object>`) y sin `sandbox` (como `<iframe>`). Su única ventaja es la simplicidad. En la práctica, para PDFs se prefiere [[02 Objeto Incrustado (object) | `<object>`]] (por el respaldo) y para páginas, [[01 Marco en Línea (iframe) | `<iframe>`]] (por la seguridad). `<embed>` queda como opción de último recurso.

## El pasado de los plugins

Como `<object>`, `<embed>` se asociaba a plugins (Flash), hoy desaparecidos. Ese uso está obsoleto.

## Buenas prácticas

> [!tip] Recomendaciones
> - En general, prefiere `<iframe>` (páginas) u `<object>` (recursos con respaldo).
> - Si lo usas, indica `type` y dimensiones.
> - No lo emplees para incrustar páginas web: carece de `sandbox`.

## Errores comunes

> [!warning] Trampas
> - **Elegirlo por costumbre** cuando `<iframe>`/`<object>` son mejores.
> - **Esperar contenido de respaldo**: no lo admite (es void).

## Notas relacionadas

- [[02 Objeto Incrustado (object)]] — con contenido de respaldo.
- [[01 Marco en Línea (iframe)]] — el estándar con `sandbox`.
