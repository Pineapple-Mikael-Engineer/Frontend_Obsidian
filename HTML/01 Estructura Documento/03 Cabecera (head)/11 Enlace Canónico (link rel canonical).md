---
title: <link rel="canonical"> — URL canónica de la página
aliases:
  - canonical link
  - rel canonical
tags:
  - html
  - api/elemento
  - metadatos
elemento: link
categoria: metadatos
rol_implicito: ninguno
vacio: true
draft: false
---

# Enlace Canónico (link rel canonical)

> [!definicion]
> `<link rel="canonical" href="…">` declara cuál es la **URL oficial** de un contenido cuando el
> mismo material es accesible desde varias direcciones. Le dice a Google qué versión indexar,
> evitando penalizaciones por contenido duplicado.

```html
<head>
  <link rel="canonical" href="https://sitio.com/articulo" />
</head>
```

## El problema que resuelve

Una misma página suele ser alcanzable por varias URLs:

| URL | Mismo contenido |
|-----|-----------------|
| `https://sitio.com/articulo` | ✅ canónica |
| `https://sitio.com/articulo?utm_source=twitter` | duplicado por parámetros de tracking |
| `https://sitio.com/articulo/` | duplicado por la barra final |
| `http://sitio.com/articulo` | duplicado por protocolo |

Sin canonical, Google ve varias páginas idénticas y reparte la autoridad entre ellas. Con canonical,
consolida todas las señales en la URL oficial.

> [!warning] Usar la URL absoluta y correcta
> El `href` canónico debe ser **absoluto** (con `https://` y dominio) y apuntar a la versión real,
> no a otra página. Un canonical mal puesto que apunte a la home hace que Google ignore la página
> concreta. Cada página se referencia a sí misma como canónica salvo que sea de verdad duplicado de
> otra.

> [!info] Es una sugerencia, no una orden
> Google trata `canonical` como una **señal fuerte pero no vinculante**: normalmente la respeta,
> pero puede elegir otra URL si detecta señales contradictorias (sitemaps, enlaces internos).

## Notas relacionadas

- [[04 Descripción (meta description)]] — otro metadato clave para buscadores.
- [[09 Metadatos y SEO/index]] — el panorama de SEO técnico.
