---
title: <blockquote> — Cita en bloque
aliases:
  - blockquote
  - block quotation
tags:
  - html
  - api/elemento
  - semantica
elemento: blockquote
categoria: flujo
rol_implicito: blockquote
vacio: false
draft: false
---

# Citas en Bloque (blockquote)

> [!definicion]
> `<blockquote>` marca una **cita larga** atribuida a otra fuente, que se separa del texto como un
> bloque propio. El navegador la sangra por defecto. Para citas cortas dentro de una frase se usa
> [[14 Citas en Línea (q) | `<q>`]].

```html
<blockquote cite="https://es.wikipedia.org/wiki/Gravedad">
  <p>Todo cuerpo perdura en su estado de reposo o de movimiento
     uniforme salvo que una fuerza actúe sobre él.</p>
</blockquote>
```

## Atributo y atribución

| Atributo | Descripción |
|----------|-------------|
| `cite` | URL de la fuente (metadato, **no visible**) |

La atribución visible no va dentro del `<blockquote>`: se coloca fuera, normalmente con
[[11 Figura (figure, figcaption) | `<figure>`/`<figcaption>`]]:

```html
<figure>
  <blockquote>
    <p>La imaginación es más importante que el conocimiento.</p>
  </blockquote>
  <figcaption>— Albert Einstein</figcaption>
</figure>
```

> [!warning] El cite no se ve
> El atributo `cite="URL"` es solo metadato para máquinas; el usuario no lo percibe. Si la fuente
> debe mostrarse, se escribe como texto (en una `<figcaption>` o un `<cite>`).

> [!info] Contenido de flujo
> `<blockquote>` admite párrafos, listas y otros bloques completos: es contenido de flujo, no solo
> texto en línea. Por eso se usa para citas de varios párrafos.

## Notas relacionadas

- [[14 Citas en Línea (q)]] — la cita corta, dentro de una frase.
- [[11 Figura (figure, figcaption)]] — para añadir la atribución visible.
