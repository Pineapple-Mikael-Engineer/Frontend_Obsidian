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
> laterales, cuadros de "ver también", publicidad, glosarios al margen, biografía del autor. Si se
> quitara, el contenido principal seguiría completo y comprensible.

```html
<main>
  <article>
    <h1>El ciclo del agua</h1>
    <p>…</p>
  </article>
  <aside aria-label="Dato curioso">
    <h2>¿Sabías que…?</h2>
    <p>Un litro de lluvia pesa exactamente un kilogramo.</p>
  </aside>
</main>
```

## Usos típicos

| Uso | Ejemplo |
|-----|---------|
| Barra lateral | Enlaces a posts relacionados, widgets |
| Cuadro destacado | Definición al margen, dato curioso, nota del editor |
| Publicidad | Banner lateral o intercalado |
| Biografía del autor | Recuadro junto a un artículo |
| Glosario contextual | Términos explicados al lado del texto |

## El criterio es semántico, no posicional

> El error central con `<aside>` es elegirlo por **dónde** va (una columna lateral) en vez de por
> **qué** es (contenido tangencial).

- Una barra lateral con la **navegación** del sitio es
  [[03 Navegación (nav) | `<nav>`]], aunque esté a un lado.
- Una columna lateral con el **contenido principal** de la página no es `<aside>` aunque el diseño la
  ponga a la derecha.
- Un `<aside>` puede estar en cualquier posición (lateral, intercalado, al final): su posición la
  decide el CSS.

## aside dentro o fuera de article

El significado de "tangencial" depende del contexto:

- **`<aside>` hijo del `<body>`/`<main>`** → tangencial a la página entera (barra lateral global).
- **`<aside>` dentro de un `<article>`** → tangencial a ese artículo concreto (una nota al margen
  relacionada solo con ese texto).

## Accesibilidad

> [!info] Landmark complementary
> `<aside>` se expone como landmark "complementary". Con varios en la página, conviene distinguirlos
> con `aria-label` para que el usuario sepa qué contiene cada uno al listarlos. Un `<aside>` sin
> contenido realmente complementario (usado solo para maquetar) ensucia el mapa de landmarks.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<aside>` solo para contenido **tangencial** real; la posición es trabajo de CSS.
> - Nómbralo con `aria-label` si hay más de uno o si su propósito no es obvio.
> - Para una columna lateral que en realidad es navegación, usa `<nav>`; para puro layout sin
>   semántica, `<div>`.

## Errores comunes

> [!warning] aside = "lo que va a la derecha"
> El malentendido más común: tratar `<aside>` como "el elemento de la barra lateral". No es
> posicional. Una sidebar de navegación es `<nav>`; una de contenido principal no es `<aside>`. La
> pregunta correcta es siempre *¿este contenido es secundario al principal?*, no *¿dónde lo coloco?*.

## Notas relacionadas

- [[06 Artículos (article)]] — el contenido principal al que un `<aside>` complementa.
- [[03 Navegación (nav)]] — no confundir una barra lateral de navegación con `<aside>`.
