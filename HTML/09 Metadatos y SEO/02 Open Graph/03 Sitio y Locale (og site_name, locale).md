---
title: "og:site_name y og:locale — Sitio e idioma"
aliases:
  - og site_name
  - og locale
tags:
  - html
  - api/concepto
  - metadatos
draft: false
---

# Sitio y Locale (og site_name, locale)

> [!definicion]
> `og:site_name` y `og:locale` completan la tarjeta social con el **nombre del sitio** y el **idioma** del contenido. No son obligatorias, pero pulen la presentación y ayudan a la internacionalización del compartido.

```html
<meta property="og:site_name" content="Recetas de la Abuela" />
<meta property="og:locale" content="es_ES" />
<meta property="og:locale:alternate" content="en_US" />
```

## og:site_name

Es el **nombre de la marca o sitio**, que algunas redes muestran sobre o bajo el título de la tarjeta. Es constante en todo el sitio (a diferencia de `og:title`, que cambia por página):

```html
<meta property="og:site_name" content="Recetas de la Abuela" />
```

## og:locale

Indica el **idioma y región** del contenido, en formato `idioma_PAÍS` (con guion bajo, no guion):

| Valor | Significado |
|-------|-------------|
| `es_ES` | Español de España |
| `es_MX` | Español de México |
| `en_US` | Inglés de EE. UU. |
| `pt_BR` | Portugués de Brasil |

> [!info] Formato distinto al de lang
> Cuidado con el formato: el atributo HTML [[01 Idioma (html lang) | `lang`]] usa guion (`es-ES`), pero `og:locale` usa **guion bajo** (`es_ES`). Es un detalle fácil de confundir.

## Versiones en otros idiomas

`og:locale:alternate` lista otros idiomas disponibles del contenido, para que la red pueda ofrecer la versión adecuada:

```html
<meta property="og:locale" content="es_ES" />
<meta property="og:locale:alternate" content="en_US" />
<meta property="og:locale:alternate" content="fr_FR" />
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Incluye `og:site_name` para reforzar la marca en cada tarjeta.
> - Declara `og:locale` con el formato `idioma_PAÍS` (guion bajo).
> - En sitios multilingües, añade `og:locale:alternate` por cada idioma.
> - Mantén coherencia con el `lang` del documento (mismo idioma, distinto formato).

## Errores comunes

> [!warning] Trampas
> - **Usar guion** (`es-ES`) en `og:locale`: el formato correcto es con guion bajo (`es_ES`).
> - **`og:site_name` distinto por página**: debe ser constante (la marca).
> - **Olvidarlas**: la tarjeta funciona sin ellas, pero queda menos pulida.

## Notas relacionadas

- [[01 Propiedades Básicas (og title, type, url)]] — las propiedades obligatorias.
- [[01 Idioma (html lang)]] — el `lang` del documento (formato con guion).
