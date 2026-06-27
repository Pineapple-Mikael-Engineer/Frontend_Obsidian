---
title: src y format — El archivo de la fuente y su formato
aliases:
  - font src
  - woff2
tags:
  - css
  - api/concepto
  - tipografia
draft: false
order: 1
---

# src y format

> [!definicion]
> El descriptor `src` de [[index | `@font-face`]] indica **dónde está** el archivo de la fuente y **en qué formato**. La pista `format()` ayuda al navegador a elegir el archivo que entiende sin descargarlo, y permite listar varios formatos como respaldo.

```css
@font-face {
  font-family: "Inter";
  src: url("/fonts/inter.woff2") format("woff2"),
       url("/fonts/inter.woff")  format("woff");
}
```

## Los formatos de fuente

| Formato | `format()` | Estado |
|---------|-----------|--------|
| **WOFF2** | `"woff2"` | El estándar actual: mejor compresión, soporte universal |
| WOFF | `"woff"` | Anterior; respaldo para navegadores muy antiguos |
| TTF / OTF | `"truetype"` / `"opentype"` | Sin comprimir para web; evitar |
| EOT, SVG | — | Obsoletos (IE / iOS antiguo) |

> [!tip] WOFF2 es casi siempre suficiente
> En la práctica moderna, basta con **WOFF2**: tiene la mejor compresión (archivos hasta un 30% más pequeños que WOFF) y soporte en todos los navegadores actuales. Listar WOFF como respaldo solo es necesario si debes soportar navegadores antiguos. Los formatos sin comprimir (TTF/OTF) no deben servirse a producción: pesan mucho más.

## El orden y format()

> [!info] El navegador toma el primero que entiende
> En la lista de `src`, el navegador prueba las fuentes **en orden** y usa la primera cuyo `format()` reconoce, **sin descargar** las demás. Por eso se pone WOFF2 primero (preferido) y WOFF después (respaldo):
> ```css
> src: url("f.woff2") format("woff2"),   /* preferido */
>      url("f.woff")  format("woff");      /* respaldo */
> ```
> Sin `format()`, el navegador tendría que descargar para averiguar si puede usar el archivo, malgastando ancho de banda.

## src: local() para usar la fuente instalada

`src` puede incluir `local("Nombre")` para usar la fuente **ya instalada** en el sistema del usuario antes de descargarla, ahorrando la descarga si la tiene:

```css
src: local("Inter"), url("/fonts/inter.woff2") format("woff2");
```

> [!warning] local() tiene riesgos de seguridad/consistencia
> `local()` puede coincidir con una fuente distinta del mismo nombre instalada por el usuario, dando un resultado inconsistente, y se ha usado para *fingerprinting*. Muchos lo omiten hoy, prefiriendo siempre descargar la versión controlada.

## Optimizar el archivo

> [!tip] Subsetting y woff2
> Para reducir el peso de la fuente:
> - **WOFF2** (compresión moderna).
> - **Subsetting**: incluir solo los caracteres que el sitio usa (p. ej. solo latín), recortando muchísimo el archivo. Se combina con [[03 unicode-range | `unicode-range`]].
> - **Precargar** la fuente crítica con `<link rel="preload" as="font" crossorigin>` para que empiece a descargar antes.

## Buenas prácticas

> [!tip] Recomendaciones
> - Sirve **WOFF2**; añade WOFF solo si necesitas navegadores antiguos.
> - Pon siempre `format()` para que el navegador elija sin descargar de más.
> - Haz *subsetting* a los caracteres que uses (latín, etc.).
> - Precarga la fuente crítica para acelerar el primer render.

## Errores comunes

> [!warning] Trampas
> - **Servir TTF/OTF** a producción: archivos enormes sin comprimir.
> - **Omitir `format()`**: descargas innecesarias.
> - **No hacer subsetting**: cargar miles de glifos que no se usan.

## Notas relacionadas

- [[index]] — la regla `@font-face` completa.
- [[02 font-display]] — qué se ve mientras la fuente descarga.
- [[03 unicode-range]] — cargar solo ciertos caracteres.
