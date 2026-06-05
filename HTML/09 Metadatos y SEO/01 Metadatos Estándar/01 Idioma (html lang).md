---
title: Idioma del documento (html lang) — desde el SEO
aliases:
  - lang seo
  - idioma del documento
tags:
  - html
  - api/concepto
  - metadatos
draft: false
---

# Idioma (html lang)

> [!definicion]
> El atributo `lang` en [[02 Elemento Raíz (html) | `<html>`]] declara el idioma principal del documento. Aunque es el cimiento de la accesibilidad, también importa para el **SEO internacional**: ayuda a los buscadores a ofrecer la página a usuarios del idioma correcto.

```html
<html lang="es">
```

## Por qué importa al SEO

| Función | Efecto |
|---------|--------|
| Segmentación por idioma | Google ofrece la versión adecuada a cada usuario |
| Traducción automática | Chrome decide si ofrecer traducir comparando `lang` con el idioma del usuario |
| Señal de calidad | Un `lang` incorrecto (página en español con `lang="en"`) confunde la indexación |

## Sitios multilingües: hreflang

> [!info] Varias versiones de idioma con hreflang
> Para un sitio con la misma página en varios idiomas, no basta `lang`: hay que decirle a Google **qué versión** mostrar a cada usuario con etiquetas `hreflang` en el `<head>`:
> ```html
> <link rel="alternate" hreflang="es" href="https://sitio.com/es/pagina" />
> <link rel="alternate" hreflang="en" href="https://sitio.com/en/page" />
> <link rel="alternate" hreflang="x-default" href="https://sitio.com/" />
> ```
> Cada versión enlaza a todas las demás (incluida a sí misma), y `x-default` marca la versión por defecto. Así Google muestra la española al usuario español y la inglesa al inglés, sin tratarlas como contenido duplicado.

## lang local para fragmentos

Un fragmento en otro idioma se marca con `lang` en el elemento concreto (atributo global), lo que ayuda a la pronunciación y al corrector:

```html
<p>El concepto de <span lang="en">deadline</span> es habitual.</p>
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Declara `lang` correcto en `<html>` (idioma real del contenido).
> - Para sitios multilingües, añade `hreflang` enlazando todas las versiones + `x-default`.
> - Marca los fragmentos en otro idioma con `lang` local.
> - Mantén coherencia: el `lang`, el contenido y el `hreflang` deben concordar.

## Errores comunes

> [!warning] Trampas
> - **`lang` heredado de una plantilla** en otro idioma.
> - **hreflang sin reciprocidad**: cada versión debe enlazar a las demás, o Google lo ignora.
> - **Olvidar `x-default`** en sitios multilingües.

## Notas relacionadas

- [[02 Elemento Raíz (html)]] — el atributo `lang` en detalle.
- [[01 HTML Semántico como Base]] — `lang` desde la accesibilidad.
- [[index]] — el resto de metadatos estándar.
