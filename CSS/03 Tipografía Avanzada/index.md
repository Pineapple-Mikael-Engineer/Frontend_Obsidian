---
title: Tipografía Avanzada — Control fino del texto
aliases:
  - advanced typography
tags:
  - css
  - api/concepto
  - tipografia
draft: false
---

# Tipografía Avanzada

> [!definicion]
> Más allá de las [[07 Propiedades de Texto/index | propiedades de texto básicas]], CSS ofrece control fino sobre la tipografía: **cargar fuentes propias** (`@font-face`), activar funciones OpenType (ligaduras, kerning), manejar el **texto multilínea** (corte de palabras, guionado, recorte), decorar (sombras, trazo) y controlar la **dirección** de escritura para cualquier idioma.

```css
@font-face {
  font-family: "Mi Fuente";
  src: url("mifuente.woff2") format("woff2");
  font-display: swap;
}
```

## Mapa de la sección

- [[01 Fuentes Web (@font-face)/index]] — cargar y servir fuentes propias.
- [[02 Propiedades Tipográficas Avanzadas/index]] — kerning, ligaduras, fuentes variables.
- [[03 Alineación Vertical (vertical-align)]] — alinear texto e inline en vertical.
- [[04 Decoración de Texto Avanzada/index]] — sombras, énfasis, trazo, subrayado fino.
- [[05 Manejo de Texto Multilínea/index]] — corte de palabras, guionado, recorte con `…`.
- [[06 Escritura y Dirección/index]] — `writing-mode`, `direction`, texto vertical y RTL.

## Dos grandes temas

> [!info] Fuentes y flujo de texto
> La sección gira en torno a dos preocupaciones:
> 1. **Las fuentes**: cargarlas (`@font-face`), elegir cómo se muestran mientras cargan (`font-display`), y aprovechar sus capacidades avanzadas (OpenType features, fuentes variables).
> 2. **El flujo del texto**: cómo se parten las palabras largas, cómo se justifica, cómo se recorta lo que no cabe, y cómo se orienta en idiomas verticales o de derecha a izquierda.

## El rendimiento de las fuentes

Las fuentes web son uno de los recursos que más afectan a la **percepción de carga**: una fuente que tarda puede dejar el texto invisible (FOIT) o provocar un salto al cambiar (FOUT). Por eso `font-display`, `unicode-range` y el formato `woff2` aparecen aquí como herramientas de rendimiento, no solo de estilo.

## Notas relacionadas

- [[07 Propiedades de Texto/index]] — la tipografía básica (sección 01).
- [[01 Fuentes Web (@font-face)/index]] — el punto de partida de esta sección.
- [[05 Manejo de Texto Multilínea/index]] — los problemas de texto que desborda.
