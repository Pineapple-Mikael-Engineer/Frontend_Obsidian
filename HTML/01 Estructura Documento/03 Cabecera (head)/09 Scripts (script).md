---
title: <script> — Incrustar o enlazar JavaScript
aliases:
  - script element
tags:
  - html
  - api/elemento
  - metadatos
elemento: script
categoria: metadatos
rol_implicito: ninguno
vacio: false
draft: false
---

# Scripts (script)

> [!definicion]
> `<script>` incrusta JavaScript inline o enlaza un archivo `.js` externo con `src`. Es el puente
> entre la estructura HTML y el comportamiento. **Dónde** se coloca y **cómo** se carga determina si
> bloquea el render.

```html
<!-- Externo, no bloqueante, se ejecuta tras parsear el HTML -->
<script src="/js/app.js" defer></script>

<!-- Inline -->
<script>
  console.log("Hola desde el documento");
</script>
```

## Atributos de carga

| Atributo | Cuándo descarga | Cuándo ejecuta | Orden |
|----------|-----------------|----------------|-------|
| (ninguno) | Inmediato, **bloquea el parseo** | Al descargar, antes de seguir | En orden |
| `defer` | En paralelo al parseo | Tras parsear el DOM, antes de `DOMContentLoaded` | En orden |
| `async` | En paralelo al parseo | En cuanto descarga (interrumpe) | Sin garantía de orden |

`type="module"` carga el script como [módulo ES] (con `import`/`export`); es `defer` por defecto.

## Dónde colocarlo

- **`defer` en el `<head>`** — el patrón moderno recomendado: descarga temprano, ejecuta cuando el
  DOM está listo, sin bloquear.
- **Al final del `<body>`** — patrón clásico equivalente: el DOM ya existe cuando el script corre.
- **Sin `defer`/`async` en mitad del documento** — bloquea: evitar.

> [!warning] Un script bloqueante congela el parseo
> Sin `defer`/`async`, el navegador detiene la construcción del DOM hasta descargar y ejecutar el
> script. Un `<script src>` pesado al inicio del `<head>` deja la página en blanco. Por eso la regla:
> `defer`, o al final del `<body>`.

> [!info] El comportamiento se delega a JavaScript
> Aquí solo se documenta **el elemento `<script>` como punto de carga**. La lógica (manipular el DOM,
> eventos, asincronía) vive en el curso de [[02 Modificar Atributos | JavaScript]]. No usar `onclick`
> ni JS inline en atributos: rompe la separación de capas.

## Notas relacionadas

- [[07 Enlace a CSS (link)]] — el equivalente para CSS.
- [[index]] — el `<head>` y el orden de carga.
