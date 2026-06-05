---
title: <hr> — Separación temática
aliases:
  - horizontal rule
  - hr
tags:
  - html
  - api/elemento
  - semantica
elemento: hr
categoria: flujo
rol_implicito: separator
vacio: true
draft: false
---

# Línea Horizontal (hr)

> [!definicion]
> `<hr>` representa un **cambio temático** a nivel de párrafo: un cambio de escena en una narración,
> un giro de tema dentro de una sección. El navegador lo dibuja como una línea horizontal, pero su
> significado es semántico, no decorativo.

```html
<p>…fin de la primera idea.</p>
<hr />
<p>Comienza un tema distinto…</p>
```

Es un elemento void. Su rol ARIA implícito es `separator`.

> [!info] De decorativo a semántico
> En HTML4, `<hr>` era una "regla horizontal" puramente visual con atributos como `width` y `align`
> (hoy obsoletos). En HTML5 se redefinió como **separación temática**: la línea es solo su
> representación por defecto.

> [!tip] Línea decorativa vs. cambio de tema
> Para una línea puramente decorativa (un divisor de diseño sin cambio de contenido), usar un borde
> CSS (`border-top`) en lugar de `<hr>`. Reservar `<hr>` para cuando de verdad cambia el tema, para
> que su rol `separator` aporte significado.

## Notas relacionadas

- [[01 Párrafos (p)]] — los bloques que `<hr>` separa.
- [[05 Secciones (section)]] — para una división más fuerte y con título.
