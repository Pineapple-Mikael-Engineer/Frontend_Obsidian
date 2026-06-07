---
title: Fuentes Web (@font-face) — Cargar fuentes propias
aliases:
  - web fonts
  - font-face
tags:
  - css
  - api/concepto
  - tipografia
draft: false
---

# Fuentes Web (@font-face)

> [!definicion]
> La regla `@font-face` permite **cargar una fuente** que no está instalada en el dispositivo del usuario, descargándola desde un archivo. Así un sitio puede usar su tipografía de marca en cualquier equipo, en lugar de limitarse a las fuentes del sistema.

```css
@font-face {
  font-family: "Inter";
  src: url("/fonts/inter.woff2") format("woff2");
  font-weight: 100 900;       /* rango (fuente variable) */
  font-display: swap;
}

body { font-family: "Inter", system-ui, sans-serif; }
```

## Cómo funciona

> [!info] Declarar y luego usar
> `@font-face` hace dos cosas separadas:
> 1. **Declara** una fuente: le da un `font-family` (un nombre interno) y dice dónde está el archivo (`src`).
> 2. Luego, ese nombre se usa en `font-family` como cualquier otra fuente.
>
> El navegador descarga el archivo **solo si** algún elemento usa esa fuente (carga diferida automática). Por eso declarar muchas `@font-face` no penaliza si no se usan todas.

## Las sub-reglas (descriptores)

| Descriptor | Para qué |
|------------|----------|
| `font-family` | El nombre con el que se referenciará |
| `src` | El archivo y su formato |
| `font-weight` | Qué peso(s) cubre este archivo |
| `font-style` | Si es normal o italic |
| `font-display` | Cómo mostrar el texto mientras carga |
| `unicode-range` | Qué caracteres cubre (carga parcial) |

El detalle de los descriptores clave: [[01 src y format]], [[02 font-display]] y [[03 unicode-range]].

## Mapa de la subsección

- [[01 src y format]] — el archivo, los formatos (`woff2`) y los respaldos.
- [[02 font-display]] — evitar el texto invisible mientras la fuente carga.
- [[03 unicode-range]] — cargar solo los caracteres que se usan.

## Fuentes propias vs. servicios

| Opción | Pros | Contras |
|--------|------|---------|
| `@font-face` con archivos propios | Control total, privacidad, sin terceros | Hay que alojar y optimizar |
| Google Fonts / Adobe Fonts | Fácil, cacheado | Dependes de un tercero, privacidad |
| Fuentes del sistema (`system-ui`) | Cero carga, nativo | Sin identidad de marca |

> [!tip] Aloja tus propias fuentes
> Hoy se recomienda **autoalojar** las fuentes (descargar el `woff2` y servirlo desde tu dominio) en vez de enlazar a Google Fonts: mejor rendimiento (sin conexión a otro dominio), mejor privacidad y control de la carga con `font-display`.

## Notas relacionadas

- [[01 src y format]] — el formato `woff2` y los respaldos.
- [[02 font-family]] — usar la fuente cargada.
- [[02 font-display]] — la estrategia de carga.
