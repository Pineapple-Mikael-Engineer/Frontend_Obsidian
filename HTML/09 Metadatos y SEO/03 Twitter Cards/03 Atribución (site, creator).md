---
title: "twitter:site y twitter:creator — Atribución a cuentas"
aliases:
  - twitter site creator
tags:
  - html
  - api/concepto
  - metadatos
draft: false
---

# Atribución (site, creator)

> [!definicion]
> `twitter:site` y `twitter:creator` vinculan la tarjeta a **cuentas de X**: la del sitio/publicación y la del autor. X las muestra en la tarjeta ("vía @sitio") y las usa para análisis. Son opcionales pero refuerzan la marca.

```html
<meta name="twitter:site" content="@RecetasAbuela" />
<meta name="twitter:creator" content="@ana_cocina" />
```

## Las dos atribuciones

| Etiqueta | A quién atribuye |
|----------|------------------|
| `twitter:site` | La cuenta del **sitio/publicación** (la marca) |
| `twitter:creator` | La cuenta del **autor** del contenido concreto |

`twitter:site` es constante en todo el sitio; `twitter:creator` cambia según quién firme cada artículo.

## El formato: con @

> [!info] Incluye el arroba
> El valor debe ser el nombre de usuario de X **con `@`** (`@cuenta`), no la URL del perfil ni el nombre sin arroba:
> ```html
> <meta name="twitter:site" content="@MiSitio" />   <!-- ✅ -->
> <meta name="twitter:site" content="MiSitio" />     <!-- ❌ -->
> ```

## Para qué sirve

- **Visibilidad**: X puede mostrar "vía @sitio" en la tarjeta, dando crédito y reconocimiento de marca.
- **Análisis**: la cuenta vinculada accede a estadísticas de cómo se comparte su contenido.
- **Autoría**: `twitter:creator` da crédito al autor individual, útil para periodistas y creadores.

## Buenas prácticas

> [!tip] Recomendaciones
> - Añade `twitter:site` con la cuenta oficial del sitio en todas las páginas.
> - Usa `twitter:creator` con la cuenta del autor en artículos firmados.
> - Incluye siempre el `@`.
> - Si el sitio o el autor no tienen cuenta en X, omite la etiqueta (no la inventes).

## Errores comunes

> [!warning] Trampas
> - **Sin `@`** o con la URL del perfil: la atribución no funciona.
> - **`twitter:creator` constante** cuando debería cambiar por autor.
> - **Cuenta inexistente**: no aporta y puede confundir.

## Notas relacionadas

- [[01 Tipo de Tarjeta (twitter card)]] — la etiqueta base de la tarjeta.
- [[02 Contenido (title, description, image)]] — el contenido de la tarjeta.
