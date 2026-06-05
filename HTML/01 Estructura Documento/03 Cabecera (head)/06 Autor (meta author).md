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
> `<meta name="author">` indica quién escribió la página. Es informativo: lo leen algunas
> herramientas de gestión documental y CMS, pero **no afecta al SEO ni al render**.

```html
<meta name="author" content="Noelia Yósselin" />
```

## Metadatos `<meta name>` informativos relacionados

| `name` | Para qué |
|--------|----------|
| `author` | Persona u organización que creó la página |
| `generator` | Software que generó el HTML (lo ponen los CMS automáticamente) |
| `copyright` | Aviso de derechos (poco usado; mejor en el `<footer>`) |
| `theme-color` | Color de la barra del navegador en móvil — **sí tiene efecto visual** |

> [!info] La autoría "fuerte" va en datos estructurados
> Para que Google asocie un artículo a su autor de forma real, se usa JSON-LD con `Person`/`author`,
> no este `<meta>`. Este se queda como nota informativa del documento.

> [!tip] theme-color sí se nota
> `<meta name="theme-color" content="#1e1e2e">` tiñe la barra de direcciones en Chrome móvil y la
> UI de PWAs. Es el `<meta name>` informativo con impacto visual real.

## Notas relacionadas

- [[10 Dirección (address)]] — el elemento del `<body>` para datos de contacto del autor.
- [[index]] — el `<head>` completo.
