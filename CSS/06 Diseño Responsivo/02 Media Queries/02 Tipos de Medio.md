---
title: Tipos de Medio — screen, print, all
aliases:
  - media types
tags:
  - css
  - api/concepto
  - responsive
draft: false
order: 2
---

# Tipos de Medio

> [!definicion]
> El **tipo de medio** de una media query indica para qué clase de dispositivo aplican los estilos: `screen` (pantallas), `print` (impresión) o `all` (todos). Va al principio de la query, antes de las features.

```css
@media screen and (min-width: 768px) { /* solo pantallas grandes */ }
@media print { /* solo al imprimir */ }
```

## Los tipos vigentes

| Tipo | Aplica a |
|------|----------|
| `all` | Todos los dispositivos (por defecto si se omite) |
| `screen` | Pantallas (monitores, móviles, tablets) |
| `print` | Impresión y vista previa de impresión |

> [!info] Los tipos antiguos están obsoletos
> Existieron tipos como `handheld`, `tv`, `projection`, `aural`… pero se **deprecaron** (eran imprecisos y poco usados). Hoy solo quedan tres vigentes: `all`, `screen` y `print`. Las distinciones por tipo de dispositivo se hacen ahora con **features** (`hover`, `pointer`, `width`), no con tipos.

## screen suele omitirse

> [!tip] Casi siempre se omite el tipo
> Como las features de ancho ya implican pantalla, el tipo `screen` se **omite** la mayoría de las veces (el valor por defecto `all` funciona igual para pantallas):
> ```css
> @media screen and (min-width: 768px) { }   /* explícito */
> @media (min-width: 768px) { }                /* equivalente, más común */
> ```
> Solo se especifica `screen` cuando quieres **excluir** la impresión de unas reglas concretas.

## @media print: los estilos de impresión

> [!tip] Optimizar la página para imprimir
> `@media print` es muy útil y a menudo olvidado: define cómo se ve la página **al imprimir**, que suele necesitar ajustes (quitar menús y banners, fondo blanco, texto negro, sin sombras):
> ```css
> @media print {
>   nav, .banner, .sidebar { display: none; }   /* ocultar lo no imprimible */
>   body { color: #000; background: #fff; }      /* tinta negra sobre blanco */
>   a::after { content: " (" attr(href) ")"; }   /* mostrar las URLs de los enlaces */
>   .articulo { break-inside: avoid; }            /* no cortar entre páginas */
> }
> ```
> Una hoja de impresión bien hecha ahorra tinta y mejora la legibilidad de las páginas impresas/guardadas como PDF.

## Cargar una hoja solo para impresión

También se puede vincular una hoja CSS completa solo para impresión con el atributo `media` del `<link>`:

```html
<link rel="stylesheet" href="print.css" media="print" />
```

El navegador la descarga con baja prioridad y solo la aplica al imprimir.

## Buenas prácticas

> [!tip] Recomendaciones
> - Omite `screen` salvo que quieras excluir explícitamente la impresión.
> - Añade un bloque `@media print` para ocultar navegación/banners y usar tinta negra.
> - Muestra las URLs de los enlaces al imprimir (`a::after { content: attr(href) }`).
> - Usa `break-inside: avoid` en impresión para no cortar tablas/figuras.

## Errores comunes

> [!warning] Trampas
> - **Usar tipos obsoletos** (`handheld`, `tv`): no funcionan; usa features.
> - **Olvidar los estilos de impresión**: páginas con menús y fondos oscuros que gastan tinta.
> - **`screen` redundante** en cada media query.

## Notas relacionadas

- [[01 Sintaxis @media]] — la estructura completa.
- [[04 break-inside]] — evitar cortes al imprimir.
- [[04 CSS Externo (link)]] — `media="print"` en el `<link>`.
