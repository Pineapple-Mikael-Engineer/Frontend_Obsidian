---
title: Metadatos Estándar — Idioma, robots, verificación, redirección
aliases:
  - standard metadata
  - metadatos estándar
tags:
  - html
  - api/concepto
  - metadatos
draft: false
---

# Metadatos Estándar

> [!definicion]
> Un conjunto de metadatos base controla cómo los buscadores y navegadores tratan la página: el idioma declarado, si se puede indexar ([[02 Robots (meta robots) | robots]]), la propiedad del sitio ([[03 Verificación de Sitio | verificación]]) y las redirecciones ([[04 Refresh y Redirección (http-equiv) | http-equiv]]).

## Mapa de la sección

- [[01 Idioma (html lang)]] — el idioma del documento, base de SEO y accesibilidad.
- [[02 Robots (meta robots)]] — indicar a los buscadores qué indexar y seguir.
- [[03 Verificación de Sitio]] — probar la propiedad ante Google, Bing, etc.
- [[04 Refresh y Redirección (http-equiv)]] — redirigir o recargar (y por qué evitarlo).

## Resumen rápido

| Metadato | Controla |
|----------|----------|
| `html lang` | Idioma del contenido |
| `meta robots` | Indexación y seguimiento de enlaces |
| `meta` de verificación | Demostrar propiedad del dominio |
| `meta http-equiv="refresh"` | Recarga/redirección automática (desaconsejado) |

## Notas relacionadas

- [[02 Elemento Raíz (html)]] — donde vive el atributo `lang`.
- [[11 Enlace Canónico (link rel canonical)]] — complementa a `robots` en la indexación.
