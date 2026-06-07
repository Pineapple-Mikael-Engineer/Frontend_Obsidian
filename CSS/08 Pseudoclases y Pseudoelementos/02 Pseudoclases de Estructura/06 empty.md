---
title: empty — Elementos sin contenido
aliases:
  - ":empty"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":empty"
grupo: pseudoclase
draft: false
---

# :empty

> [!definicion]
> `:empty` selecciona elementos que **no tienen contenido**: ni hijos, ni texto. Es útil para **ocultar** contenedores vacíos que de otro modo dejarían un hueco (un panel de mensajes sin mensajes, una lista sin items).

```css
.notificaciones:empty { display: none; }   /* ocultar si no hay nada dentro */
```

## Ocultar contenedores vacíos

> [!tip] El uso principal: que lo vacío no estorbe
> El caso más común: un contenedor que a veces tiene contenido y a veces no (rellenado por JS). Cuando está vacío, su padding/borde/margen dejan un **hueco feo**. `:empty` permite ocultarlo automáticamente:
> ```css
> .alertas:empty { display: none; }
> .resultados:empty::after { content: "Sin resultados"; }   /* o mostrar un mensaje */
> ```

## Qué cuenta como "vacío"

> [!warning] El espacio en blanco cuenta como contenido
> El detalle traicionero: `:empty` es **estricto**. Un elemento con **espacios en blanco**, saltos de línea o un comentario **no** se considera vacío:
> ```html
> <div></div>          <!-- :empty SÍ -->
> <div> </div>         <!-- :empty NO (hay un espacio) -->
> <div>
> </div>               <!-- :empty NO (hay un salto de línea) -->
> <div><!-- x --></div><!-- :empty SÍ (los comentarios no cuentan)... -->
> ```
> Históricamente, cualquier espacio rompía `:empty`. Las versiones modernas son algo más permisivas con el espacio en blanco en algunos casos, pero la regla segura es: **sin nada dentro** (lo que genera el HTML/JS debe estar realmente vacío).

## Combinar con :not(:empty)

> [!tip] Estilar solo cuando SÍ hay contenido
> `:not(:empty)` selecciona lo contrario: elementos **con** contenido. Útil para aplicar estilos (padding, borde) solo cuando hay algo que mostrar:
> ```css
> .badge:not(:empty) {
>   padding: 0.2rem 0.5rem;
>   background: red;
>   border-radius: 9999px;   /* solo se ve el badge si tiene contenido */
> }
> ```
> Así un badge de notificación solo aparece (con su fondo y padding) cuando tiene un número dentro.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `:empty` para ocultar contenedores vacíos (`display: none`) y evitar huecos.
> - Usa `:not(:empty)` para aplicar estilos solo cuando hay contenido (badges).
> - Recuerda que **el espacio en blanco cuenta**: el elemento debe estar realmente vacío.
> - Asegúrate de que el JS no deje espacios/saltos dentro de los contenedores que quieres tratar como vacíos.

## Errores comunes

> [!warning] Trampas
> - **`:empty` que no coincide** porque hay un espacio o salto de línea dentro.
> - **Confundir `:empty` con `:only-child`**: sin contenido vs. sin hermanos.
> - **Plantillas que dejan espacios** rompiendo la detección de vacío.

## Notas relacionadas

- [[04 not()]] — `:not(:empty)` para el caso contrario.
- [[05 only-child y only-of-type]] — sin hermanos (distinto de sin contenido).
- [[02 Propiedad content]] — mostrar un mensaje en lo vacío con `::after`.
