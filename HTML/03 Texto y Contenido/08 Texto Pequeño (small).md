---
title: <small> — Texto secundario (legal, anotaciones)
aliases:
  - small
  - small print
tags:
  - html
  - api/elemento
  - semantica
elemento: small
categoria: fraseo
rol_implicito: ninguno
vacio: false
draft: false
---

# Texto Pequeño (small)

> [!definicion]
> `<small>` representa **comentarios secundarios** o la "letra pequeña": avisos legales, derechos de
> autor, descargos de responsabilidad, anotaciones. El navegador lo muestra un punto más pequeño,
> pero su significado es "información accesoria", no solo "texto reducido".

```html
<footer>
  <small>&copy; 2026 Mi Sitio. Precios sin IVA.</small>
</footer>
```

## Usos correctos

| Uso | Ejemplo |
|-----|---------|
| Copyright | `<small>© 2026</small>` |
| Descargo legal | "Resultados no garantizados" |
| Atribución | Fuente de una cita o imagen |

> [!warning] No para reducir tamaño arbitrario
> `<small>` no es "hacer el texto pequeño". Para eso está `font-size` en CSS. Reservarlo para
> contenido que es semánticamente **secundario** (legal, anotación). Un titular reducido por diseño
> no es `<small>`.

> [!info] No afecta a la importancia
> `<small>` no resta importancia semántica al texto (no es lo contrario de
> [[04 Énfasis Fuerte (strong) | `<strong>`]]); solo lo marca como comentario lateral. Puede
> contener énfasis y enlaces como cualquier texto.

## Notas relacionadas

- [[09 Pie de Sección (footer)]] — la ubicación habitual del copyright en `<small>`.
- [[index]] — el resto de elementos de texto.
