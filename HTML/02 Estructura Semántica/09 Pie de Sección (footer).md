---
title: <footer> — Pie de página o de sección
aliases:
  - footer element
tags:
  - html
  - api/elemento
  - semantica
elemento: footer
categoria: flujo
rol_implicito: contentinfo
vacio: false
draft: false
---

# Pie de Sección (footer)

> [!definicion]
> `<footer>` agrupa el contenido de **cierre** de su contexto: copyright, enlaces legales y de
> contacto si es de la página; autor, fecha o referencias si es de un
> [[06 Artículos (article) | `<article>`]]. Es la contraparte de
> [[08 Cabecera de Sección (header) | `<header>`]].

```html
<footer>
  <p>&copy; 2026 Mi Sitio. Todos los derechos reservados.</p>
  <nav aria-label="Legal">
    <a href="/privacidad">Privacidad</a>
    <a href="/terminos">Términos</a>
  </nav>
</footer>
```

## Contenido típico

| Contexto | Contenido |
|----------|-----------|
| Pie de página (`<body>`) | Copyright, enlaces legales, redes sociales, contacto |
| Pie de artículo | Autor, fecha, etiquetas, referencias |

> [!info] Landmark "contentinfo"
> `<footer>` se expone como landmark `contentinfo` **solo** cuando es hijo directo del `<body>`.
> Anidado en un `<article>` o `<section>`, es un pie local sin rol de landmark, igual que el
> `<header>`.

> [!tip] El contacto va en address
> Los datos de contacto dentro de un `<footer>` se marcan con
> [[10 Dirección (address) | `<address>`]], no con un `<p>` cualquiera. Así el navegador y los
> lectores los reconocen como información de contacto.

## Notas relacionadas

- [[08 Cabecera de Sección (header)]] — su contraparte de apertura.
- [[10 Dirección (address)]] — para los datos de contacto del pie.
