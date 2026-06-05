---
title: <meta author> — Autor del documento
aliases:
  - meta author
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

# Autor (meta author)

> [!definicion]
> `<meta name="author">` indica quién escribió la página. Es puramente informativo: lo leen algunas
> herramientas de gestión documental y CMS, pero **no afecta al SEO ni al renderizado**. Forma parte
> de una familia de `<meta name>` informativos que conviene conocer en conjunto.

```html
<meta name="author" content="Noelia Yósselin" />
```

## La familia de meta name informativos

El atributo `name` admite muchos valores estandarizados o convencionales. Algunos son solo
informativos; otros **sí tienen efecto visual**:

| `name` | Para qué | ¿Efecto visible? |
|--------|----------|------------------|
| `author` | Persona u organización que creó la página | No |
| `generator` | Software que generó el HTML (lo ponen los CMS solos) | No |
| `copyright` | Aviso de derechos | No (mejor en el `<footer>`) |
| `theme-color` | Color de la barra del navegador en móvil | **Sí** |
| `color-scheme` | Indica si la página soporta modo claro/oscuro | **Sí** |
| `referrer` | Política de envío de la cabecera Referer | No (afecta a red) |

### theme-color: el informativo con impacto real

```html
<meta name="theme-color" content="#1e1e2e" />
```

Tiñe la barra de direcciones en Chrome móvil y la interfaz de las PWA instaladas. Puede declararse
con `media` para adaptarse al esquema del sistema:

```html
<meta name="theme-color" media="(prefers-color-scheme: light)" content="#ffffff" />
<meta name="theme-color" media="(prefers-color-scheme: dark)"  content="#1e1e2e" />
```

## Dónde vive la autoría "real"

> [!info] Para SEO, la autoría va en datos estructurados
> Para que Google asocie de verdad un artículo a su autor (y lo muestre en resultados enriquecidos),
> se usa **JSON-LD** con un objeto `Person` en el campo `author`, no este `<meta>`. La etiqueta
> `meta author` se queda como nota informativa del documento; el peso semántico lo lleva
> [[03 Person y Organization | el schema Person]].

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `meta author` si tu flujo documental lo aprovecha; si no, no es imprescindible.
> - **Sí** añade `theme-color` (mejora la integración visual en móvil) y `color-scheme` si soportas
>   modo oscuro.
> - Los datos de contacto del autor que el usuario debe ver van en
>   [[10 Dirección (address) | `<address>`]] dentro del `<body>`, no aquí.

## Notas relacionadas

- [[10 Dirección (address)]] — el elemento visible para datos de contacto del autor.
- [[index]] — el resto de metadatos del `<head>`.
