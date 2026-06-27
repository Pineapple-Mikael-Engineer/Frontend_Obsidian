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
order: 8
---

# Texto Pequeño (small)

> [!definicion]
> `<small>` representa **comentarios secundarios** o la "letra pequeña": avisos legales, derechos de autor, descargos de responsabilidad, atribuciones, anotaciones al margen. El navegador lo muestra un punto más pequeño, pero su significado es "información accesoria", no simplemente "texto reducido".

```html
<footer>
  <small>&copy; 2026 Mi Sitio. Precios sin IVA. Sujeto a disponibilidad.</small>
</footer>
```

## Usos correctos

| Uso | Ejemplo |
|-----|---------|
| Copyright | `<small>© 2026 Empresa</small>` |
| Descargo legal | "Resultados no garantizados. Consulte condiciones." |
| Atribución | Fuente de una cita o de una imagen |
| Nota aclaratoria menor | "*disponible solo en España" |

## Significado, no tamaño

El criterio para `<small>` es **semántico**: marca contenido que es secundario respecto a lo que lo rodea. No es "hacer el texto pequeño" —para eso está `font-size` en CSS— sino "esto es la letra pequeña del contenido".

```html
<!-- ✅ small: es contenido legal secundario -->
<p>Oferta válida hasta agotar existencias.
   <small>Consulte las bases en el establecimiento.</small></p>

<!-- ❌ small para reducir un titular por diseño: usa CSS -->
<h2><small>Subtítulo reducido</small></h2>
```

## No afecta a la importancia

> [!info] small no es lo contrario de strong
> `<small>` no resta importancia semántica al texto ni lo "desenfatiza": solo lo marca como comentario lateral. Puede contener énfasis, enlaces y cualquier elemento de fraseo. Un aviso legal puede ser pequeño y a la vez contener un `<strong>` con la parte crítica.

## Buenas prácticas

> [!tip] Recomendaciones
> - Reserva `<small>` para contenido genuinamente secundario (legal, atribuciones, anotaciones).
> - Para reducir tamaño por diseño sin que el contenido sea secundario, usa `font-size` en CSS.
> - El copyright del pie es su caso de uso por excelencia.

## Errores comunes

> [!warning] small como herramienta de tamaño
> Usar `<small>` para encoger cualquier texto (un subtítulo, un pie de foto grande) es confundir presentación con significado. Si el texto no es "letra pequeña" en sentido de contenido secundario, su tamaño es asunto del CSS.

## Notas relacionadas

- [[09 Pie de Sección (footer)]] — la ubicación habitual del copyright en `<small>`.
- [[10 Dirección (address)]] — los datos de contacto, a veces dentro de `<small>` en el pie.
