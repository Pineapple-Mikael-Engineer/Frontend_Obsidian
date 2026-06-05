---
title: <body> — Contenido visible del documento
aliases:
  - body element
tags:
  - html
  - api/elemento
  - estructura
elemento: body
categoria: seccionado
rol_implicito: ninguno
vacio: false
draft: false
---

# Cuerpo (body)

> [!definicion]
> `<body>` contiene **todo el contenido que el usuario ve**: texto, imágenes, formularios, enlaces,
> vídeos. Es el segundo y último hijo de [[02 Elemento Raíz (html) | `<html>`]], después de
> [[03 Cabecera (head)/index | `<head>`]]. Solo puede haber **uno** por documento, y representa la
> frontera entre lo que se procesa (head) y lo que se muestra.

```html
<body>
  <header>…</header>
  <main>…</main>
  <footer>…</footer>
</body>
```

Lo que vive fuera del `<body>` no se renderiza como contenido; vive en el `<head>` como metadato. La
única excepción histórica es el [[03 Título del Documento (title) | `<title>`]], que el usuario ve en
la pestaña aunque resida en el `<head>`.

## Cómo organizar su contenido

Un `<body>` bien formado no es una pila de `<div>`: se estructura con
[[02 Estructura Semántica/index | landmarks semánticos]] que dan navegación gratuita a las
tecnologías de asistencia.

### Esqueleto recomendado

```html
<body>
  <header>
    <a href="/" class="logo">Mi Sitio</a>
    <nav aria-label="Principal">…</nav>
  </header>

  <main>
    <h1>Título de la página</h1>
    <article>…</article>
  </main>

  <footer>
    <p>&copy; 2026 Mi Sitio.</p>
  </footer>
</body>
```

Este patrón —[[08 Cabecera de Sección (header) | `<header>`]],
[[04 Contenido Principal (main) | `<main>`]], [[09 Pie de Sección (footer) | `<footer>`]]— se repite
en casi cualquier página y es el punto de partida idiomático.

### Tolerancia del parser

El `<body>` es **opcional en el código fuente**: si se escribe contenido sin abrirlo, el parser lo
crea implícitamente y mueve dentro todo lo que sea contenido de flujo. Por eso una página "funciona"
sin `<body>` explícito. Aun así, escribirlo deja la intención clara y evita sorpresas con el árbol
DOM resultante.

## Eventos asociados

El `<body>` es el destino histórico de varios eventos de la ventana. Hoy se prefiere registrarlos
desde JavaScript sobre `window` o `document` en lugar de con atributos inline:

| Evento | Se dispara cuando | Dónde escucharlo hoy |
|--------|-------------------|----------------------|
| `load` | Toda la página y sus recursos (imágenes, CSS) cargaron | `window` |
| `DOMContentLoaded` | El DOM está parseado, sin esperar imágenes | `document` |
| `beforeunload` | El usuario va a abandonar la página | `window` |
| `pageshow` / `pagehide` | Navegación con caché de ida-vuelta (bfcache) | `window` |

```js
// Patrón moderno, no usar onload="..." inline en <body>
document.addEventListener("DOMContentLoaded", () => {
  // el DOM ya existe; seguro manipularlo
});
```

## Estilos del body

`<body>` suele recibir estilos base que se heredan a todo el documento: tipografía, color de texto y
fondo, márgenes. El navegador le aplica un `margin: 8px` por defecto que casi todos los *resets* CSS
eliminan:

```css
body {
  margin: 0;
  font-family: system-ui, sans-serif;
  line-height: 1.6;
  color: #1e1e2e;
  background: #fff;
}
```

> [!info] body vs. html para el fondo
> El color de fondo tiene una peculiaridad: si solo `<body>` tiene `background`, el navegador lo
> **propaga** a toda la ventana (incluso más allá del alto del body). Para fondos con `height: 100%`
> o degradados a pantalla completa conviene tener presente esta interacción entre `<html>` y
> `<body>`.

## Buenas prácticas

> [!tip] Recomendaciones
> - Estructura el contenido con landmarks (`header`/`main`/`footer`), no con `<div>` genéricos.
> - Coloca el `<script>` con `defer` o al final del `<body>` para no bloquear el render.
> - Aplica los estilos base heredables (tipografía, color) en `body`, no repetidos por elemento.

## Errores comunes

> [!warning] Lo que no va en el body
> - **Estilos inline** (`style="…"`) ni **manejadores inline** (`onclick="…"`): la presentación es
>   CSS y el comportamiento es JS. Mantén separadas las tres capas. Inline solo para ilustrar la mala
>   práctica.
> - **Más de un `<body>`**: solo uno por documento; un segundo se ignora o corrompe el árbol.
> - **Contenido huérfano** sin landmark: texto suelto bajo `<body>` sin `<main>` deja a los lectores
>   de pantalla sin punto de salto al contenido principal.

## Notas relacionadas

- [[03 Cabecera (head)/index]] — su contraparte invisible.
- [[02 Estructura Semántica/index]] — cómo organizar el contenido del body con landmarks.
- [[09 Scripts (script)]] — dónde y cómo cargar JavaScript dentro del body.
