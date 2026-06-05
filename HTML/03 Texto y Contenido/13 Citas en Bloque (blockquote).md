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
> `<blockquote>` marca una **cita larga** atribuida a otra fuente, que se separa del texto como un bloque propio. El navegador la sangra por defecto. Para citas cortas dentro de una frase se usa [[14 Citas en Línea (q) | `<q>`]].

```html
<blockquote cite="https://es.wikipedia.org/wiki/Gravedad">
  <p>Todo cuerpo perdura en su estado de reposo o de movimiento uniforme
     salvo que una fuerza actúe sobre él.</p>
</blockquote>
```

## Contenido de flujo

A diferencia de `<q>` (en línea), `<blockquote>` admite **contenido de bloque completo**: párrafos, listas, varios `<p>`, incluso otras citas. Por eso es la herramienta para citas de varios párrafos:

```html
<blockquote>
  <p>Primer párrafo de la cita.</p>
  <p>Segundo párrafo de la misma cita.</p>
</blockquote>
```

## El atributo cite (y por qué no basta)

| Atributo | Descripción | ¿Visible? |
|----------|-------------|-----------|
| `cite` | URL de la fuente de la cita | No (solo metadato) |

> [!warning] cite no se ve
> El atributo `cite="URL"` es metadato para máquinas: el usuario **no lo percibe**. Si quieres mostrar la fuente, hay que escribirla como texto. La atribución visible **no va dentro** del `<blockquote>` (eso la convertiría en parte de la cita): se coloca fuera.

## Atribución correcta con figure

El patrón recomendado para cita + autor visible usa [[11 Figura (figure, figcaption) | `<figure>`/`<figcaption>`]]:

```html
<figure>
  <blockquote>
    <p>La imaginación es más importante que el conocimiento.</p>
  </blockquote>
  <figcaption>— Albert Einstein, <cite>Sobre la ciencia</cite></figcaption>
</figure>
```

El elemento `<cite>` dentro de la atribución marca el **título de la obra** citada (no el nombre del autor, según la especificación estricta).

## Estilo

El sangrado por defecto se suele reemplazar por un diseño con borde lateral:

```css
blockquote {
  margin-inline: 0;
  padding-inline-start: 1rem;
  border-inline-start: 4px solid #cba6f7;
  font-style: italic;
}
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<blockquote>` para citas de bloque (una o varias líneas/párrafos) de una fuente externa.
> - La atribución visible va **fuera** del `<blockquote>`, idealmente en una `<figcaption>`.
> - Añade `cite="URL"` como metadato, aunque no se vea.
> - Para una cita corta dentro de una frase, usa `<q>`.

## Errores comunes

> [!warning] Trampas
> - **Meter la atribución dentro**: "—Einstein" dentro del `<blockquote>` pasa a formar parte de la cita.
> - **Usar `<blockquote>` solo para sangrar** texto que no es una cita: el sangrado es CSS, no semántica de cita.
> - **Olvidar el `<p>` interno**: el texto suele ir en párrafos dentro del bloque, no suelto.

## Notas relacionadas

- [[14 Citas en Línea (q)]] — la cita corta, dentro de una frase.
- [[11 Figura (figure, figcaption)]] — para añadir la atribución visible a la cita.
