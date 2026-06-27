---
title: Tablas Responsivas — Adaptar tablas a pantallas estrechas
aliases:
  - responsive tables
tags:
  - css
  - api/concepto
  - responsive
draft: false
order: 4
---

# Tablas Responsivas

> [!definicion]
> Las **tablas** son uno de los elementos más difíciles de adaptar a móvil: tienen un ancho mínimo (las columnas no se pueden apretar sin hacer ilegible el contenido). Hay varias estrategias, cada una con sus ventajas y costes de accesibilidad.

```css
/* La más simple y robusta: scroll horizontal */
.tabla-scroll { overflow-x: auto; }
```

## Estrategia 1: scroll horizontal (la más segura)

> [!tip] Envolver la tabla en un contenedor con scroll
> La opción **más robusta y accesible**: envolver la [[01 Contenedor de Tabla (table) | tabla]] en un contenedor que permita desplazamiento horizontal en móvil. La tabla mantiene su estructura intacta:
> ```css
> .tabla-wrap { overflow-x: auto; }
> ```
> ```html
> <div class="tabla-wrap" tabindex="0" role="region" aria-label="Datos">
>   <table>…</table>
> </div>
> ```
> El usuario desliza horizontalmente para ver las columnas. **Preserva la semántica** de la tabla (la asociación celda-encabezado sigue funcionando), a diferencia de reorganizarla. El `tabindex="0"` permite hacer scroll con teclado.

## Estrategia 2: reorganizar a tarjetas (con cuidado)

> [!warning] Convertir filas en tarjetas rompe la semántica
> Una técnica popular reestructura cada fila como una "tarjeta" en móvil, con `display: block` y los encabezados repetidos vía `data-*` y `::before`:
> ```css
> @media (max-width: 600px) {
>   table, tbody, tr, td { display: block; }
>   td::before { content: attr(data-label); font-weight: bold; }
> }
> ```
> **El problema**: cambiar el `display` de los elementos de tabla **destruye la semántica** (la asociación celda-encabezado para lectores de pantalla se pierde, la tabla deja de anunciarse como tabla). Es vistoso pero **menos accesible**. Úsalo solo si el scroll no encaja, y prueba con lector de pantalla.

## Estrategia 3: mostrar menos columnas

Para tablas con muchas columnas, ocultar las menos importantes en móvil (con un toggle para verlas) reduce el ancho:

```css
@media (max-width: 600px) {
  .col-secundaria { display: none; }   /* ocultar columnas menos críticas */
}
```

## Comparación

| Estrategia | Accesibilidad | Esfuerzo | Cuándo |
|------------|---------------|----------|--------|
| Scroll horizontal | **Alta** (preserva la tabla) | Bajo | Por defecto, casi siempre |
| Tarjetas | Baja (rompe semántica) | Alto | Datos simples, con pruebas a11y |
| Ocultar columnas | Media | Medio | Tablas muy anchas |

## Buenas prácticas

> [!tip] Recomendaciones
> - Por defecto, **scroll horizontal**: simple, robusto y accesible.
> - Da `tabindex="0"` y un `role="region"` con `aria-label` al contenedor con scroll.
> - Evita la reorganización a tarjetas salvo necesidad, y pruébala con lector de pantalla.
> - Para tablas muy anchas, considera ocultar columnas secundarias en móvil.

## Errores comunes

> [!warning] Trampas
> - **Reorganizar a tarjetas** sin valorar la pérdida de accesibilidad.
> - **Tabla que desborda** sin contenedor con scroll: rompe el layout de la página.
> - **Sin `tabindex`** en el contenedor con scroll: no se puede desplazar con teclado.

## Notas relacionadas

- [[01 Contenedor de Tabla (table)]] — la tabla y su scroll responsivo.
- [[03 Celda de Encabezado (th)]] — la asociación que la reorganización rompe.
- [[12 Atributos de Datos (data-*)]] — los `data-label` del truco de tarjetas.
