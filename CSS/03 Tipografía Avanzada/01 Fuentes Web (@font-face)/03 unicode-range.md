---
title: unicode-range — Cargar solo ciertos caracteres
aliases:
  - unicode-range
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: unicode-range
grupo: tipografia
valor_inicial: "U+0-10FFFF"
hereda: false
animable: false
draft: false
order: 3
---

# unicode-range

> [!definicion]
> El descriptor `unicode-range` de [[index | `@font-face`]] limita **qué caracteres** cubre un archivo de fuente. El navegador solo descarga ese archivo si la página usa caracteres dentro del rango. Permite partir una fuente en subconjuntos (latín, cirílico, griego…) y cargar solo los necesarios.

```css
@font-face {
  font-family: "Inter";
  src: url("inter-latin.woff2") format("woff2");
  unicode-range: U+0000-00FF;   /* solo latín básico */
}
```

## La sintaxis del rango

| Forma | Significa |
|-------|-----------|
| `U+0041` | Un único punto de código (la "A") |
| `U+0000-00FF` | Un rango (latín básico) |
| `U+04??` | Comodín: todo `U+0400`–`U+04FF` (cirílico) |

Se pueden listar varios separados por comas.

## La carga por subconjuntos

> [!info] Una fuente, varios archivos por idioma
> La técnica habitual (la que usa Google Fonts) parte la fuente en **archivos por subconjunto** —latín, latín extendido, cirílico, griego, vietnamita…—, cada uno con su `@font-face` y su `unicode-range`. El navegador descarga **solo** los archivos cuyos caracteres aparecen en la página:
> ```css
> @font-face { font-family: F; src: url("f-latin.woff2") format("woff2");
>   unicode-range: U+0000-00FF; }
> @font-face { font-family: F; src: url("f-cyrillic.woff2") format("woff2");
>   unicode-range: U+0400-04FF; }
> ```
> Una página solo en español descarga el archivo latín y **nunca** el cirílico, ahorrando peso.

## Carga bajo demanda real

> [!tip] Solo se descarga si el carácter aparece
> Lo potente: el archivo de un subconjunto se descarga **únicamente si la página contiene** algún carácter de su rango. Si insertas un nombre en cirílico en una página por lo demás latina, el navegador descarga el subconjunto cirílico **en ese momento**. Es carga perezosa de glifos, automática.

## Relación con el subsetting manual

`unicode-range` reparte la carga entre varios archivos; el **subsetting** (recortar la fuente a ciertos glifos al generar el `woff2`) reduce cada archivo. Se combinan: subconjuntos por idioma, cada uno ya recortado a sus glifos. Detalle del archivo en [[01 src y format | src y format]].

## Buenas prácticas

> [!tip] Recomendaciones
> - Parte fuentes multi-idioma en subconjuntos con `unicode-range`.
> - Para un sitio en un solo idioma, sirve solo ese subconjunto.
> - Combínalo con subsetting del archivo para minimizar el peso.
> - Si autoalojas, genera los subconjuntos con herramientas como `glyphhanger` o `fonttools`.

## Errores comunes

> [!warning] Trampas
> - **Rangos solapados** entre `@font-face`: descargas duplicadas.
> - **No incluir caracteres usados** (acentos, `€`, comillas tipográficas): se ven con la fuente de respaldo, descuadrando.
> - **Cargar todos los subconjuntos** cuando solo usas latín: peso innecesario.

## Notas relacionadas

- [[01 src y format]] — el archivo de cada subconjunto.
- [[02 font-display]] — cómo se muestra mientras cargan.
- [[index]] — la regla `@font-face`.
