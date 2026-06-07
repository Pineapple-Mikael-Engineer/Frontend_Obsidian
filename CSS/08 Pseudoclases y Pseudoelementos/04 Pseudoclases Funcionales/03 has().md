---
title: has() — Seleccionar por descendiente o relación
aliases:
  - ":has"
  - selector de padre
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":has"
grupo: pseudoclase
draft: false
---

# :has()

> [!definicion]
> `:has(selector)` selecciona un elemento **según lo que contiene o lo que le sigue**: coincide si tiene un descendiente (o un hermano, con combinadores) que casa con el selector. Es el famoso **"selector de padre"**, que durante décadas se pidió y que solo se podía emular con JavaScript.

```css
.card:has(img) { padding: 0; }              /* tarjetas que contienen una imagen */
form:has(:invalid) { border-color: #f38ba8; } /* formularios con algún campo inválido */
```

## Seleccionar hacia arriba

> [!tip] El padre reacciona al hijo
> Lo revolucionario: CSS siempre seleccionó **hacia abajo** (`.padre .hijo` estila el hijo). `:has()` permite estilar el **padre** según el hijo:
> ```css
> /* Una tarjeta sin imagen tiene más padding que una con imagen */
> .card { padding: 1.5rem; }
> .card:has(img) { padding: 0; }
>
> /* Un label cuyo campo es requerido lleva asterisco */
> label:has(+ input:required)::after { content: " *"; color: red; }
> ```

## Combinado con combinadores

> [!info] No solo descendientes: hermanos también
> `:has()` admite combinadores dentro, lo que permite relaciones laterales:
> | Selector | Coincide con… |
> |----------|---------------|
> | `.x:has(img)` | `.x` que **contiene** una `<img>` |
> | `.x:has(> img)` | `.x` con una `<img>` hija **directa** |
> | `.x:has(+ .y)` | `.x` **seguido** de un `.y` |
> | `h2:has(+ p)` | un `<h2>` seguido de un `<p>` |
>
> Esto habilita ajustar el espaciado entre elementos según qué viene después, algo antes imposible en CSS.

## Lo que antes requería JavaScript

> [!tip] Patrones que ya no necesitan JS
> `:has()` reemplaza muchos casos de JavaScript que solo añadían/quitaban clases según el contenido:
> ```css
> /* Layout distinto según el número de hijos */
> .galeria:has(:nth-child(4)) { grid-template-columns: repeat(2, 1fr); }
>
> /* Resaltar el formulario al haber un error tras enviar */
> form:has(:user-invalid) .resumen-errores { display: block; }
>
> /* Modo "con sidebar" automático */
> .layout:has(.sidebar) { grid-template-columns: 250px 1fr; }
>
> /* Estilar el body si hay un modal abierto */
> body:has(.modal[open]) { overflow: hidden; }
> ```

## Cuidado con el rendimiento y la especificidad

> [!warning] :has() es potente pero no gratis
> Dos cautelas:
> - **Especificidad**: como [[01 is() | `:is()`]], `:has()` toma la especificidad del selector más alto de su argumento (el elemento al que se aplica también cuenta).
> - **Rendimiento**: `:has()` obliga al navegador a evaluar relaciones complejas; selectores `:has()` muy amplios o anidados pueden ser costosos. En la práctica está muy optimizado, pero evita abusar de `:has()` globales (`:has(...)` sin un elemento que lo acote).

## Soporte

`:has()` tiene **buen soporte** en todos los navegadores modernos (desde 2023). Como mejora progresiva: el diseño debe funcionar sin él, y `:has()` añade refinamientos donde hay soporte.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `:has()` para estilar padres según su contenido o relaciones laterales.
> - Reemplaza con `:has()` los casos donde JS solo añadía clases según el DOM.
> - Acota el selector (`.card:has(...)`, no `:has(...)` global) por rendimiento.
> - Trátalo como mejora progresiva si soportas navegadores antiguos.

## Errores comunes

> [!warning] Trampas
> - **`:has()` demasiado amplio** sin acotar: posible coste de rendimiento.
> - **Especificidad inesperada** por el argumento de `:has()`.
> - **Depender de él sin respaldo** en navegadores antiguos.

## Notas relacionadas

- [[05 focus-within]] — un caso particular ("padre con descendiente enfocado").
- [[01 is()]] · [[04 not()]] — otras funcionales que combina.
- [[index]] — el impacto de `:has()` en CSS.
