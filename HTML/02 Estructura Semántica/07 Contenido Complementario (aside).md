---
title: <aside> — Contenido tangencial o complementario
aliases:
  - aside element
  - sidebar
tags:
  - html
  - api/elemento
  - semantica
elemento: aside
categoria: seccionado
rol_implicito: complementary
vacio: false
draft: false
---

# Contenido Complementario (aside)

> [!definicion]
> `<aside>` marca contenido **relacionado pero secundario** respecto al contenido principal: barras
> laterales, cuadros de "ver también", publicidad, glosarios al margen. Si se quitara, el contenido
> principal seguiría completo.

```html
<main>
  <article>
    <h1>El ciclo del agua</h1>
    <p>…</p>
  </article>
  <aside>
    <h2>Sabías que…</h2>
    <p>Un litro de lluvia pesa exactamente un kilogramo.</p>
  </aside>
</main>
```

## Usos típicos

| Uso | Ejemplo |
|-----|---------|
| Barra lateral | Enlaces a posts relacionados |
| Cuadro destacado | Definición al margen, dato curioso |
| Publicidad | Banner lateral |
| Biografía del autor | Junto a un artículo |

> [!info] Landmark "complementary"
> `<aside>` se expone como landmark "complementary" a las tecnologías de asistencia. Con varios en
> la página, distinguirlos con `aria-label`.

> [!warning] No es para cualquier cosa a un lado
> El criterio es **semántico**, no visual: `<aside>` significa "tangencial al contenido", no
> "colocado en una columna lateral". Una barra lateral con la navegación principal sigue siendo
> [[03 Navegación (nav) | `<nav>`]], no `<aside>`. La posición es trabajo de CSS.

## Notas relacionadas

- [[06 Artículos (article)]] — el contenido principal al que un `<aside>` complementa.
- [[03 Navegación (nav)]] — no confundir una barra lateral de navegación con `<aside>`.
