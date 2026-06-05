---
title: <nav> — Bloque de navegación
aliases:
  - nav element
  - navigation
tags:
  - html
  - api/elemento
  - semantica
elemento: nav
categoria: seccionado
rol_implicito: navigation
vacio: false
draft: false
---

# Navegación (nav)

> [!definicion]
> `<nav>` marca un bloque de **enlaces de navegación principales**: el menú del sitio, la tabla de
> contenidos, la paginación. No todo grupo de enlaces es un `<nav>`; solo los conjuntos de
> navegación significativos.

```html
<nav aria-label="Principal">
  <ul>
    <li><a href="/">Inicio</a></li>
    <li><a href="/blog">Blog</a></li>
    <li><a href="/contacto">Contacto</a></li>
  </ul>
</nav>
```

## Cuándo sí y cuándo no

| Usar `<nav>` | No usar `<nav>` |
|--------------|-----------------|
| Menú principal del sitio | Un enlace suelto en un párrafo |
| Tabla de contenidos | Enlaces dentro de un artículo |
| Paginación | El listado de enlaces del `<footer>` legal (basta el `<footer>`) |
| Breadcrumbs | — |

> [!info] Landmark de navegación
> `<nav>` expone un **landmark "navigation"** a los lectores de pantalla, que pueden listarlo y
> saltar a él. Si hay varios `<nav>` (principal, de pie, breadcrumb), distinguirlos con
> `aria-label` o `aria-labelledby` para que el usuario sepa cuál es cuál.

> [!warning] No abusar
> Envolver cada lista de enlaces en `<nav>` satura los landmarks y resta utilidad a la navegación
> asistida. Reservarlo para la navegación **primaria**. El contenido va dentro con una
> [[02 Listas No Ordenadas (ul) | lista `<ul>`]], que es la estructura idiomática de un menú.

## Notas relacionadas

- [[02 Listas No Ordenadas (ul)]] — la estructura interna típica de un menú.
- [[01 Enlaces (a)]] — los enlaces que contiene.
- [[01 Roles de Landmark]] — `<nav>` como landmark de accesibilidad.
