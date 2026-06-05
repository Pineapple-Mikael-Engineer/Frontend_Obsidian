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
> `<address>` marca la **información de contacto** del autor o propietario del documento o del
> [[06 Artículos (article) | artículo]] más cercano: correo, teléfono, dirección física, enlaces a
> redes. No es para cualquier dirección postal citada en el texto.

```html
<footer>
  <address>
    Escrito por <a href="mailto:ana@sitio.com">Ana López</a>.<br />
    Calle Mayor 1, Madrid.
  </address>
</footer>
```

## Qué sí y qué no

| Sí es `<address>` | No es `<address>` |
|-------------------|-------------------|
| Contacto del autor del artículo | La dirección de un local mencionada en una noticia |
| Contacto de la empresa en el pie | Una dirección postal dentro de un párrafo narrativo |

El criterio: `<address>` responde "¿cómo contactar a **quien escribe**?", no "¿qué direcciones
aparecen en el contenido?".

> [!warning] Solo contenido de contacto
> El contenido válido es información de contacto, no arbitrario. Anidar un artículo entero o datos
> no relacionados dentro de `<address>` es incorrecto. Su ámbito es el `<article>` o `<body>` que lo
> contiene.

> [!info] Renderizado por defecto
> Los navegadores muestran `<address>` en **cursiva** por defecto (heredando el estilo de
> `<i>`/`<em>`). Es presentación, ajustable con CSS; lo importante es el significado.

## Notas relacionadas

- [[09 Pie de Sección (footer)]] — la ubicación habitual de `<address>`.
- [[03 Enlaces a Correo (mailto)]] — el enlace de correo que suele contener.
