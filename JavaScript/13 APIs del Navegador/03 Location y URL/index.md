---
title: Location y URL — visión general
aliases:
  - Location y URL
  - window.location
  - URL API
tags:
  - javascript
  - api/objeto
  - browser
draft: false
order: 3
---

# Location y URL

> [!definicion]
> El navegador expone dos mecanismos complementarios para trabajar con URLs: **`window.location`**, que refleja y controla la URL de la página actual (lectura, navegación y recarga), y la clase **`URL`** (ES2016), que proporciona un parser completo para analizar y construir URLs arbitrarias. Junto a ellos, **`URLSearchParams`** facilita la lectura y modificación ergonómica de los parámetros de consulta (query string) sin necesidad de manipulación manual de strings.

```js
// Leer la URL actual con location
console.log(location.href);       // "https://app.ejemplo.com/perfil?id=42#bio"
console.log(location.pathname);   // "/perfil"
console.log(location.search);     // "?id=42"

// Parsear una URL arbitraria con la clase URL
const url = new URL(location.href);
console.log(url.hostname);        // "app.ejemplo.com"
url.searchParams.get("id");       // "42"
```

## Contenido de esta sección

- [[01 Propiedades de location|Propiedades de location]] — todas las partes de `window.location` (href, protocol, host, pathname, search, hash, origin) y la clase `URL`.
- [[02 reload, assign, replace|reload, assign, replace]] — los tres métodos de navegación: recarga, navegación con historial y redirección sin historial.
- [[03 URLSearchParams|URLSearchParams]] — API para leer, modificar y serializar query strings de forma ergonómica.

## Por qué importa

Manipular la URL directamente con `location` o `URL` elimina la necesidad de regex frágiles para parsear partes de la dirección. Más importante aún, combinado con la History API (`pushState`/`popstate`), permite que las SPAs actualicen la URL visible para el usuario sin recargar la página, lo que habilita compartir URLs, navegación con botones de atrás/adelante y SEO correcto.

> [!tip]
> La clase `URL` es la forma moderna de parsear y construir URLs. Prefiere `new URL(href)` sobre manipulación manual de strings o regex siempre que necesites extraer o modificar partes de una dirección, aunque no sea la URL actual de la página.

> [!warning]
> `location.href = url` y `location.assign(url)` son equivalentes y ambos añaden entrada al historial. Para redirecciones donde el usuario no debe poder volver (login → dashboard, página de error temporal), usar `location.replace(url)` para no contaminar el historial.

## Notas relacionadas

- [[../04 History/index|History API]] — `pushState`, `replaceState` y `popstate` para routing de SPA sin recargar
- [[../../07 Programación Asíncrona/index|Programación Asíncrona]] — fetch desde la URL actual o construida con `URL`
- [[../../08 Comunicación con APIs/index|Comunicación con APIs]] — construcción de endpoints con `URL` y `URLSearchParams`
