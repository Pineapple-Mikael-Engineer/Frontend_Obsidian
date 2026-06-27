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
order: 9
---

# Pie de Sección (footer)

> [!definicion]
> `<footer>` agrupa el contenido de **cierre** de su contexto: como pie de página, el copyright, los
> enlaces legales y el contacto; como pie de un `<article>`, el autor, la fecha, las etiquetas o las
> referencias. Es la contraparte simétrica de `<header>`.

```html
<footer>
  <p>&copy; 2026 Mi Sitio. Todos los derechos reservados.</p>
  <nav aria-label="Legal">
    <a href="/privacidad">Privacidad</a>
    <a href="/terminos">Términos</a>
  </nav>
</footer>
```

## Dos roles según el contexto

Igual que [[08 Cabecera de Sección (header) | `<header>`]], su significado depende de su ubicación:

| Ubicación | Contenido típico | ¿Landmark? |
|-----------|------------------|------------|
| Hijo directo del `<body>` | Copyright, legal, redes, contacto | Sí, `contentinfo` |
| Dentro de `<article>` / `<section>` | Autor, fecha, etiquetas, referencias | No, pie local |

## Qué suele contener

- **Pie de página**: aviso de copyright, enlaces a privacidad/términos, mapa del sitio, redes
  sociales, datos de contacto.
- **Pie de artículo**: autor, fecha de publicación, etiquetas, "artículos relacionados", contador de
  comentarios.

El contacto del autor o de la empresa se marca con [[10 Dirección (address) | `<address>`]], no con
un `<p>` cualquiera:

```html
<footer>
  <address>
    Contacto: <a href="mailto:hola@sitio.com">hola@sitio.com</a>
  </address>
  <p><small>&copy; 2026 Mi Sitio.</small></p>
</footer>
```

## Accesibilidad

> [!info] Landmark contentinfo
> `<footer>` se expone como landmark "contentinfo" **solo** cuando es hijo directo del `<body>`.
> Anidado en un `<article>` o `<section>`, es un pie local sin rol de landmark, igual que el
> `<header>`. Debe haber un único "contentinfo" por página (el pie principal del sitio).

## Buenas prácticas

> [!tip] Recomendaciones
> - Un `<footer>` de página (contentinfo) por documento, hijo directo del `<body>`.
> - Envuelve los datos de contacto en `<address>`.
> - Los enlaces legales pueden ir en un `<nav aria-label="Legal">`, aunque no es obligatorio.
> - Reutiliza `<footer>` dentro de cada `<article>` para sus metadatos de cierre.

## Errores comunes

> [!warning] Trampas
> - **Confundir el alcance**: un `<footer>` dentro de un `<article>` es del artículo, no de la
>   página; no genera "contentinfo".
> - **Datos de contacto sin `<address>`**: pierde la semántica de "información de contacto".
> - **Varios pies de página banner/contentinfo**: solo uno por documento a nivel de `<body>`.

## Notas relacionadas

- [[08 Cabecera de Sección (header)]] — su contraparte de apertura.
- [[10 Dirección (address)]] — para los datos de contacto dentro del pie.
