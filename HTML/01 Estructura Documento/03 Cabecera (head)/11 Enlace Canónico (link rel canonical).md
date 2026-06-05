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
> `<link rel="canonical" href="…">` declara cuál es la **URL oficial** de un contenido cuando el mismo
> material es accesible desde varias direcciones. Le dice a Google qué versión indexar y dónde
> consolidar la autoridad, evitando que el contenido duplicado se reparta las señales de
> posicionamiento.

```html
<head>
  <link rel="canonical" href="https://sitio.com/articulo" />
</head>
```

## El problema que resuelve

Una misma página suele ser alcanzable por muchas URLs sin que el autor lo pretenda:

| URL | Por qué duplica |
|-----|-----------------|
| `https://sitio.com/articulo` | ✅ la canónica |
| `https://sitio.com/articulo?utm_source=twitter` | parámetros de campaña/tracking |
| `https://sitio.com/articulo/` | barra final |
| `http://sitio.com/articulo` | protocolo sin HTTPS |
| `https://www.sitio.com/articulo` | subdominio `www` |
| `https://sitio.com/articulo?orden=fecha` | parámetros de orden/filtrado |

Sin canonical, Google ve varias páginas idénticas, no sabe cuál es la principal y **reparte** la
autoridad de enlaces entre ellas, debilitando el posicionamiento de todas.

## Casos de uso típicos

- **Parámetros de URL** (tracking, filtros, paginación) que no cambian el contenido esencial.
- **Contenido sindicado**: si republicas un artículo en otro sitio, el canónico apunta al original
  para que el crédito SEO vaya allí.
- **Versiones imprimible/AMP**: apuntan a la versión canónica HTML.
- **Auto-referencia**: incluso una página sin duplicados conocidos suele declararse canónica de sí
  misma, como práctica defensiva.

## Reglas de uso correcto

> [!warning] URL absoluta y exacta
> - El `href` debe ser **absoluto** (`https://` + dominio completo), no relativo.
> - Debe apuntar a la versión **real y accesible** (código 200), no a una redirección ni a un 404.
> - Cada página se referencia **a sí misma** salvo que sea de verdad un duplicado de otra.
>
> Un canonical mal puesto que apunte siempre a la home hace que Google **ignore** las páginas
> concretas y solo indexe la portada: un error de SEO costoso y difícil de notar.

## Sugerencia, no orden

> [!info] Señal fuerte pero no vinculante
> Google trata `canonical` como una **señal fuerte**, no como una directiva obligatoria. Normalmente
> la respeta, pero puede elegir otra URL como canónica si detecta señales contradictorias (sitemaps,
> enlaces internos, redirecciones). Para forzar de verdad, se combina con redirecciones 301 y un
> sitemap coherente.

## Buenas prácticas

> [!tip] Recomendaciones
> - Declara canonical **auto-referencial** en todas las páginas indexables.
> - Mantén coherencia entre canonical, sitemap, redirecciones y enlaces internos: que todos apunten a
>   la misma versión (con/sin `www`, con/sin barra final).
> - En e-commerce con filtros, canonicaliza las variantes de filtrado hacia la categoría base.

## Errores comunes

> [!warning] Trampas
> - **Canonical a la home en todas las páginas** (error de plantilla): desindexa el sitio entero
>   salvo la portada.
> - **Canonical relativo** o a una URL que redirige: confunde a Google.
> - **Mezclar `www` y no-`www`** entre canonical y el resto de señales: elige una y sé consistente.

## Notas relacionadas

- [[04 Descripción (meta description)]] — otro metadato clave para buscadores.
- [[09 Metadatos y SEO/index]] — el panorama del SEO técnico (incluye Open Graph y JSON-LD).
