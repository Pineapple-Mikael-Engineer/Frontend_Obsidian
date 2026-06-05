---
title: Skip Links — Saltar al contenido principal
aliases:
  - skip links
  - skip to content
tags:
  - html
  - api/concepto
  - a11y
draft: false
---

# Skip Links

> [!definicion]
> Un **skip link** ("saltar al contenido") es un enlace, normalmente el **primero** de la página, que permite a los usuarios de teclado **saltarse** la navegación repetida e ir directo al contenido principal. Suele estar oculto visualmente y aparecer solo al recibir el foco.

```html
<body>
  <a href="#contenido" class="skip-link">Saltar al contenido</a>
  <header>… navegación larga …</header>
  <main id="contenido" tabindex="-1">…</main>
</body>
```

## El problema que resuelve

> [!info] La navegación repetida es agotadora por teclado
> Un usuario de ratón ignora el menú y va directo al contenido. Pero un usuario de **teclado** tiene que pulsar `Tab` por **cada** enlace del menú —que se repite en todas las páginas— antes de llegar al contenido. En un sitio con 30 enlaces de navegación, son 30 tabulaciones en cada página. El skip link salta todo eso de un golpe.

## Cómo funciona

1. Es un [[02 Anclas Internas (id) | enlace de ancla]] (`href="#contenido"`) al `id` del [[04 Contenido Principal (main) | `<main>`]].
2. Debe ser el **primer** elemento enfocable del `<body>`.
3. Al activarlo, el foco salta al contenido principal.

El `tabindex="-1"` en el `<main>` asegura que pueda recibir el foco programáticamente al saltar (un `<main>` no es enfocable por defecto).

## Oculto, pero visible al enfocar

El patrón habitual es ocultarlo visualmente y mostrarlo solo cuando recibe el foco (al tabular), para no estorbar el diseño pero estar disponible:

```css
.skip-link {
  position: absolute;
  left: -9999px;        /* fuera de pantalla */
}
.skip-link:focus {
  left: 1rem; top: 1rem;  /* visible al enfocar */
}
```

> [!warning] No lo ocultes con display: none
> Ocultarlo con `display: none` o `visibility: hidden` lo hace **inalcanzable** también por teclado: deja de existir para el foco. Hay que sacarlo de la vista con técnicas que lo mantengan enfocable (posición fuera de pantalla, `clip`), no eliminarlo.

## Buenas prácticas

> [!tip] Recomendaciones
> - Pon un skip link como **primer** elemento enfocable de la página.
> - Apúntalo al `id` del `<main>` (con `tabindex="-1"` en el main).
> - Ocúltalo con técnica que lo mantenga enfocable; muéstralo en `:focus`.
> - Puedes añadir varios (saltar a navegación, a búsqueda) en sitios complejos.

## Errores comunes

> [!warning] Trampas
> - **Ocultarlo con `display: none`**: deja de ser enfocable.
> - **Destino sin `tabindex="-1"`**: el `<main>` no recibe el foco al saltar.
> - **No ponerlo el primero**: pierde su propósito (saltar antes de la navegación).

## Notas relacionadas

- [[04 Contenido Principal (main)]] — el destino habitual del skip link.
- [[02 Anclas Internas (id)]] — el mecanismo de enlace por `#id`.
- [[03 Contenido Oculto Visualmente (sr-only)]] — técnicas de ocultar manteniendo accesibilidad.
