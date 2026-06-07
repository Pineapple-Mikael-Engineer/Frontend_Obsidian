---
title: first-of-type y last-of-type — El primero/último de su tipo
aliases:
  - ":first-of-type"
  - ":last-of-type"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":first-of-type"
grupo: pseudoclase
draft: false
---

# :first-of-type y :last-of-type

> [!definicion]
> `:first-of-type` selecciona el **primer elemento de su tipo** entre sus hermanos; `:last-of-type`, el último. A diferencia de [[01 first-child y last-child | `:first-child`]], no exige ser el primer hijo absoluto, solo el primero de **su etiqueta**.

```css
p:first-of-type { font-size: 1.2rem; }   /* el primer <p>, aunque haya un <h2> antes */
```

## La diferencia con first-child

> [!info] De su tipo, no el primero absoluto
> El contraste, con un ejemplo:
> ```html
> <article>
>   <h2>Título</h2>
>   <p>Primer párrafo</p>   <!-- :first-of-type SÍ; :first-child NO -->
>   <p>Segundo párrafo</p>
> </article>
> ```
> - `p:first-child` → **no** coincide (el primer hijo es el `<h2>`).
> - `p:first-of-type` → coincide con el **primer `<p>`**, saltándose el `<h2>`.
>
> `:first-of-type` "ignora" los elementos de otros tipos y cuenta solo los del mismo. Es lo que quieres cuando el primer hijo suele ser un encabezado u otro elemento distinto.

## Casos de uso

```css
/* El primer párrafo de un artículo, como entradilla, aunque haya un título antes */
article p:first-of-type {
  font-size: 1.15rem;
  font-weight: 500;
}

/* La primera imagen de una galería con tamaño distinto */
.galeria img:first-of-type { grid-column: span 2; }

/* Quitar margen superior del primer elemento de cada tipo */
section h2:first-of-type { margin-top: 0; }
```

## Útil cuando el primer hijo es un encabezado

> [!tip] El patrón "título + primer párrafo"
> El caso más frecuente: quieres estilar el **primer párrafo** de un bloque (como entradilla destacada), pero ese bloque empieza con un `<h2>`. `:first-child` fallaría; `:first-of-type` es la herramienta correcta:
> ```css
> .post p:first-of-type::first-letter { font-size: 3em; }   /* capitular en el primer párrafo */
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `:first-of-type` cuando quieras el primero de una etiqueta y antes pueda haber otros elementos.
> - Es el complemento de `:first-child` para el patrón "título seguido de párrafos".
> - Combínalo con pseudoelementos (`::first-letter`) para entradillas con capitular.
> - Para una posición concreta de un tipo, usa [[04 nth-of-type() | `:nth-of-type()`]].

## Errores comunes

> [!warning] Trampas
> - **Confundirlo con `:first-child`**: uno cuenta por tipo, el otro por posición absoluta.
> - **Esperar que cuente todos los hermanos**: solo cuenta los del mismo tipo.

## Notas relacionadas

- [[01 first-child y last-child]] — la versión por posición absoluta.
- [[04 nth-of-type()]] — una posición concreta de un tipo.
- [[03 first-letter]] — la capitular del primer párrafo.
