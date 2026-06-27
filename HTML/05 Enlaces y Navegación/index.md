---
title: Enlaces y Navegación — Conectar documentos y recursos
aliases:
  - links
  - hipervínculos
tags:
  - html
  - api/concepto
  - navegacion
draft: false
order: 5
---

# Enlaces y Navegación

> [!definicion]
> El **hipervínculo** es la idea fundacional de la web: un enlace conecta un documento con otro recurso (otra página, una sección, un correo, un teléfono, un archivo). El elemento [[01 Enlaces (a) | `<a>`]] es el que materializa todas esas conexiones a través de su atributo `href`.

```html
<a href="https://ejemplo.com">Ir a Ejemplo</a>
<a href="#seccion-2">Saltar a la sección 2</a>
<a href="mailto:hola@sitio.com">Escríbenos</a>
```

## Tipos de destino según href

El mismo elemento `<a>` enlaza a destinos muy distintos según el valor de `href`:

| Destino | `href` empieza por |
|---------|--------------------|
| Otra página (absoluta) | `https://`, `//` |
| Página del propio sitio (relativa) | `/ruta`, `pagina.html` |
| Sección de la página | `#id` |
| Correo electrónico | `mailto:` |
| Teléfono | `tel:` |
| Descarga | cualquiera + `download` |

Cada destino tiene su nota: [[01 Enlaces (a)]] (páginas y descargas), [[02 Anclas Internas (id)]] (secciones), [[03 Enlaces a Correo (mailto)]] y [[04 Enlaces Telefónicos (tel)]].

## Mapa de la sección

- [[01 Enlaces (a)]] — el elemento `<a>` y todos sus atributos (`href`, `target`, `rel`, `download`…).
- [[02 Anclas Internas (id)]] — saltar a una sección dentro de la misma página con `#id`.
- [[03 Enlaces a Correo (mailto)]] — abrir el cliente de correo con `mailto:`.
- [[04 Enlaces Telefónicos (tel)]] — iniciar una llamada con `tel:`.

## Rutas absolutas vs. relativas

Una distinción transversal a todos los enlaces:

| Tipo | Ejemplo | Se resuelve respecto a |
|------|---------|------------------------|
| Absoluta | `https://sitio.com/blog/post` | El dominio completo |
| Relativa a la raíz | `/blog/post` | La raíz del dominio actual |
| Relativa al documento | `post.html`, `../img/foto.jpg` | La URL de la página actual |

Las rutas relativas hacen el sitio portable (funciona en cualquier dominio); las absolutas son necesarias para recursos externos y para canónicas/Open Graph.

## La navegación como estructura

> [!info] Enlaces + listas + nav
> Un menú de navegación combina tres piezas de esta y otras secciones: el landmark [[03 Navegación (nav) | `<nav>`]] que declara "esto es navegación", una [[02 Listas No Ordenadas (ul) | lista `<ul>`]] que agrupa los destinos, y los `<a>` que enlazan cada uno. Esa combinación es el patrón idiomático de todo menú.

## Notas relacionadas

- [[01 Enlaces (a)]] — el corazón de la sección.
- [[03 Navegación (nav)]] — agrupar enlaces de navegación principal.
