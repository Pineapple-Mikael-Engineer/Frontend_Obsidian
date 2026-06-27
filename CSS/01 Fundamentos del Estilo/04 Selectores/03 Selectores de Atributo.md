---
title: Selectores de Atributo — Apuntar por atributos y sus valores
aliases:
  - attribute selectors
tags:
  - css
  - api/selector
  - arquitectura
selector: atributo
draft: false
order: 1
---

# Selectores de Atributo

> [!definicion]
> Los selectores de **atributo** apuntan a elementos según los atributos HTML que tienen y, opcionalmente, sus valores. Van entre corchetes `[ ]` y permiten coincidencias exactas, parciales, por prefijo, sufijo o subcadena.

```css
[disabled]            { opacity: 0.5; }       /* tiene el atributo */
[type="email"]        { }                     /* valor exacto */
a[href^="https"]      { }                      /* empieza por https */
img[src$=".svg"]      { }                      /* termina en .svg */
```

## Catálogo de operadores

| Selector | Coincide si el atributo… | Ejemplo |
|----------|--------------------------|---------|
| `[attr]` | Existe (cualquier valor) | `[required]` |
| `[attr="valor"]` | Es **exactamente** ese valor | `[type="text"]` |
| `[attr~="valor"]` | Contiene esa **palabra** (en lista separada por espacios) | `[class~="btn"]` |
| `[attr\|="valor"]` | Es el valor o empieza por `valor-` | `[lang\|="es"]` |
| `[attr^="valor"]` | **Empieza** por ese valor | `[href^="https"]` |
| `[attr$="valor"]` | **Termina** en ese valor | `[href$=".pdf"]` |
| `[attr*="valor"]` | **Contiene** esa subcadena | `[href*="ejemplo"]` |

## Los más útiles en la práctica

```css
/* Enlaces externos */
a[href^="http"]:not([href*="midominio.com"]) { }

/* Enlaces a PDF, marcar con icono */
a[href$=".pdf"]::after { content: " 📄"; }

/* Estilar inputs por tipo */
input[type="checkbox"] { }

/* Atributos de datos */
[data-estado="activo"] { border-color: green; }
```

## Sensibilidad a mayúsculas

> [!info] El flag i para ignorar mayúsculas
> Por defecto, la coincidencia de valores distingue mayúsculas (salvo en atributos que el HTML define como insensibles). Se puede forzar la insensibilidad con el flag `i` antes del corchete de cierre:
> ```css
> a[href$=".PDF" i] { }   /* coincide con .pdf, .PDF, .Pdf… */
> ```

## Especificidad de clase

> [!info] Pesa como una clase
> Un selector de atributo tiene la **misma especificidad que una clase** (media). Esto los hace una alternativa razonable a las clases para estilar por estado (`[disabled]`, `[aria-current="page"]`, `[data-*]`) sin inflar la especificidad.

## Buenas prácticas

> [!tip] Recomendaciones
> - Úsalos para estilar por estado real del DOM (`[disabled]`, `[aria-*]`, `[data-*]`).
> - `^=`, `$=`, `*=` son potentes para enlaces (externos, PDF, dominios concretos).
> - El flag `i` para coincidencias insensibles a mayúsculas (extensiones de archivo).
> - Prefiere atributos ARIA/data semánticos a hacks frágiles.

## Errores comunes

> [!warning] Trampas
> - **Olvidar las comillas** en valores con caracteres especiales o espacios.
> - **Confundir `~=` (palabra) con `*=` (subcadena)**: `[class~="btn"]` busca la palabra exacta, no un fragmento.
> - **Escapar mal** caracteres dentro del valor.

## Notas relacionadas

- [[02 Selector de Clase]] — misma especificidad, otro enfoque.
- [[05 Estados ARIA (expanded, hidden, checked, selected)]] — atributos ARIA para estilar estados.
- [[12 Atributos de Datos (data-*)]] — `data-*` como gancho de estilo.
