---
title: Incrustación — Insertar contenido externo
aliases:
  - embedding
  - incrustación
tags:
  - html
  - api/concepto
  - multimedia
draft: false
order: 3
---

# Incrustación

> [!definicion]
> La incrustación inserta en el documento contenido que vive **fuera de él**: otra página web, un PDF, un mapa, un vídeo de YouTube. El elemento principal es [[01 Marco en Línea (iframe) | `<iframe>`]]; [[02 Objeto Incrustado (object) | `<object>`]] y [[03 Incrustación Genérica (embed) | `<embed>`]] son alternativas más antiguas y de uso reducido.

```html
<iframe src="https://www.openstreetmap.org/export/embed.html"
        title="Mapa de la ubicación" width="600" height="450"></iframe>
```

## Mapa de la sección

- [[01 Marco en Línea (iframe)]] — el elemento estándar para incrustar páginas y servicios.
- [[02 Objeto Incrustado (object)]] — incrustación genérica (PDF, recursos).
- [[03 Incrustación Genérica (embed)]] — incrustación de recursos externos (legado).

## Cuál usar

| Necesidad | Elemento |
|-----------|----------|
| Incrustar una página, mapa, vídeo de terceros | `<iframe>` |
| Mostrar un PDF u otro recurso con respaldo | `<object>` |
| Incrustar un recurso externo simple | `<embed>` (poco usado) |

En la práctica, `<iframe>` cubre casi todos los casos modernos; `<object>` y `<embed>` quedan para situaciones puntuales.

## El tema transversal: seguridad

> [!warning] Incrustar contenido ajeno es un riesgo
> Meter contenido de otro origen en tu página tiene implicaciones de seguridad: el contenido incrustado podría intentar acciones no deseadas. Por eso `<iframe>` ofrece el atributo `sandbox` (restringe lo que el contenido puede hacer) y `allow` (permisos explícitos), y el servidor controla quién puede incrustarte con cabeceras como `X-Frame-Options`/`Content-Security-Policy`. Es el hilo conductor de la sección, desarrollado en [[01 Marco en Línea (iframe) | `<iframe>`]].

## Notas relacionadas

- [[01 Marco en Línea (iframe)]] — el elemento central y su seguridad.
- [[01 Lienzo (canvas)]] · [[02 Gráficos Vectoriales (svg)]] — para dibujar, no incrustar.
