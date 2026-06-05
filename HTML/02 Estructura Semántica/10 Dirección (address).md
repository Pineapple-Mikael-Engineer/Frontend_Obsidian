---
title: <address> — Datos de contacto
aliases:
  - address element
tags:
  - html
  - api/elemento
  - semantica
elemento: address
categoria: flujo
rol_implicito: ninguno
vacio: false
draft: false
---

# Dirección (address)

> [!definicion]
> `<address>` marca la **información de contacto** del autor o propietario del documento, o del
> `<article>` más cercano: correo, teléfono, dirección física, enlaces a perfiles. **No** es para
> cualquier dirección postal que aparezca citada en el texto.

```html
<footer>
  <address>
    Escrito por <a href="mailto:ana@sitio.com">Ana López</a>.<br />
    Estudio López · Calle Mayor 1, Madrid.
  </address>
</footer>
```

## Qué sí y qué no es address

El malentendido habitual es creer que `<address>` sirve para "direcciones postales". En realidad
marca **cómo contactar a quien es responsable** del contenido:

| Sí es `<address>` | No es `<address>` |
|-------------------|-------------------|
| Contacto del autor del artículo | La dirección de un local mencionada en una noticia |
| Contacto/empresa en el pie de página | Una dirección postal dentro de un párrafo narrativo |
| Datos de contacto de soporte | La sede de una empresa citada como dato del texto |

La pregunta: *¿esto responde a "cómo contactar al responsable de esta página/artículo"?* Si describe
una ubicación dentro del contenido (la dirección de un restaurante reseñado), es texto normal, no
`<address>`.

## Alcance

El alcance de `<address>` es el `<article>` o `<body>` que lo contiene:

- Dentro de un `<article>` → contacto del autor de ese artículo.
- Dentro del `<footer>` de la página → contacto del sitio.

## Contenido permitido

Solo información de contacto: nombre, correo (`<a href="mailto:">`), teléfono
(`<a href="tel:">`), enlaces a redes, dirección física. **No** debe contener contenido arbitrario,
encabezados ni un artículo completo.

## Accesibilidad y estilo

> [!info] Renderizado por defecto
> Los navegadores muestran `<address>` en **cursiva** (heredando el estilo de `<i>`/`<em>`). Es
> presentación, ajustable con CSS (`address { font-style: normal; }`). Lo importante no es la cursiva
> sino el significado: las tecnologías y los buscadores reconocen el bloque como datos de contacto.

## Buenas prácticas

> [!tip] Recomendaciones
> - Colócalo en el `<footer>` de la página para el contacto del sitio, o en el `<footer>` del
>   `<article>` para el del autor.
> - Enlaza el correo con [[03 Enlaces a Correo (mailto) | `mailto:`]] y el teléfono con
>   [[04 Enlaces Telefónicos (tel) | `tel:`]].
> - Si el estilo cursiva no encaja, normalízalo con CSS sin renunciar al elemento.

## Errores comunes

> [!warning] Usarlo para cualquier dirección
> El error clásico: marcar con `<address>` la dirección de un negocio que se reseña, o la sede de una
> empresa citada como dato. Eso es contenido, no información de contacto del autor. `<address>` es
> específicamente "cómo contactar a quien firma".

## Notas relacionadas

- [[09 Pie de Sección (footer)]] — la ubicación habitual de `<address>`.
- [[03 Enlaces a Correo (mailto)]] — el enlace de correo que suele contener.
- [[04 Enlaces Telefónicos (tel)]] — el enlace de teléfono de contacto.
