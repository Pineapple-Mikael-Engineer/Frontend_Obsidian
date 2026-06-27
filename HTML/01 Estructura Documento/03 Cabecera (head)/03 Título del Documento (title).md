---
title: <title> — Título del documento
aliases:
  - title element
  - page title
tags:
  - html
  - api/elemento
  - metadatos
elemento: title
categoria: metadatos
rol_implicito: ninguno
vacio: false
draft: false
order: 3
---

# Título del Documento (title)

> [!definicion]
> `<title>` define el **título de la página**: el texto que aparece en la pestaña del navegador, en
> los marcadores, en el historial y como **titular azul** clicable en los resultados de Google. Es el
> único elemento del [[index | `<head>`]] cuyo contenido el usuario percibe directamente, y es
> **obligatorio** en un documento válido.

```html
<head>
  <title>Tarta de manzana fácil — Recetas de la Abuela</title>
</head>
```

Solo admite texto plano: las etiquetas anidadas se ignoran o se muestran literalmente.

## Dónde se usa este texto

Un mismo `<title>` alimenta varios lugares a la vez, y por eso compensa redactarlo bien:

| Lugar | Cómo se muestra |
|-------|-----------------|
| Pestaña del navegador | Truncado a pocos caracteres |
| Resultado de búsqueda (Google) | Titular azul clicable, ~60 caracteres |
| Marcador / favorito | Nombre por defecto que se guarda |
| Al compartir sin Open Graph | Título de la tarjeta en redes |
| Lector de pantalla | **Lo primero** que se anuncia al cargar la página |

## Cómo redactarlo

### El patrón "Página — Sitio"

La convención más usada antepone lo específico de la página y cierra con la marca:

```html
<title>Cómo regar un cactus — Blog de jardinería</title>
<title>Contacto — Estudio López</title>
```

Lo específico va **primero** porque la pestaña y Google truncan por la derecha: el dato útil debe
sobrevivir al recorte.

### Longitud

Google corta el titular alrededor de los **50–60 caracteres**. Pasarse no penaliza, pero el exceso no
se ve. Conviene que la idea principal quepa en ese margen.

### Unicidad

Cada página debe tener un `<title>` **único**. Páginas distintas con el mismo título confunden al
usuario (pestañas idénticas, marcadores indistinguibles) y diluyen el SEO.

## Peso en SEO y accesibilidad

> [!info] Doble función crítica
> El `<title>` es uno de los factores de SEO *on-page* más fuertes que existen, y a la vez lo primero
> que un lector de pantalla lee al abrir la página. Un buen título orienta tanto al algoritmo de
> búsqueda como a la persona ciega que navega entre veinte pestañas. Pocos elementos rinden tanto por
> tan poco.

## Título dinámico

En aplicaciones de una sola página (SPA), el `<title>` se actualiza por JavaScript al navegar entre
vistas, para que pestaña e historial reflejen la sección actual:

```js
document.title = "Carrito (3) — Mi Tienda";
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Pon lo específico al principio y la marca al final.
> - Mantén la parte clave bajo ~60 caracteres.
> - Un título único por página; nada de "Inicio" repetido en todo el sitio.
> - En SPAs, actualiza `document.title` en cada cambio de ruta.

## Errores comunes

> [!warning] No confundir con el h1
> [[01 Encabezados Jerárquicos (h1-h6) | `<h1>`]] vive en el `<body>` (titular visible de la página);
> `<title>` vive en el `<head>` (metadato, pestaña, buscador). Suelen parecerse pero **no son
> iguales**: el `<title>` añade el nombre del sitio y se optimiza para el buscador, mientras que el
> `<h1>` es el encabezado de contenido. Dejar la pestaña con el título por defecto del framework
> ("React App", "Document") es un descuido frecuente.

## Notas relacionadas

- [[04 Descripción (meta description)]] — el texto gris que acompaña al título en los buscadores.
- [[01 Encabezados Jerárquicos (h1-h6)]] — el titular visible, que no debe confundirse con el title.
