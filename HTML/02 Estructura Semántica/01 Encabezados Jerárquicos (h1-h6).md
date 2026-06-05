---
title: <h1>–<h6> — Encabezados jerárquicos
aliases:
  - headings
  - h1 h2 h3
tags:
  - html
  - api/elemento
  - semantica
elemento: h1
categoria: flujo
rol_implicito: heading
vacio: false
draft: false
---

# Encabezados Jerárquicos (h1-h6)

> [!definicion]
> `<h1>` a `<h6>` son los **títulos** de la página, ordenados por importancia: `<h1>` es el más
> relevante, `<h6>` el menos. No son seis tamaños de letra: son seis **niveles de jerarquía** que
> construyen el esquema del documento, el equivalente al índice de un libro.

```html
<h1>Guía de jardinería</h1>
  <h2>Plantas de interior</h2>
    <h3>Riego</h3>
    <h3>Luz</h3>
  <h2>Plantas de exterior</h2>
    <h3>Riego</h3>
```

La sangría no existe en el HTML real (los encabezados no se anidan unos dentro de otros); aquí solo
ilustra la jerarquía lógica que el nivel de cada uno expresa.

## El esquema del documento

Cada encabezado abre una sección implícita que pertenece al encabezado de nivel superior anterior. La
secuencia de niveles forma un árbol:

```
h1  Guía de jardinería
├── h2  Plantas de interior
│   ├── h3  Riego
│   └── h3  Luz
└── h2  Plantas de exterior
    └── h3  Riego
```

Este árbol es lo que un lector de pantalla anuncia y por el que el usuario navega. Una jerarquía bien
formada **es** la tabla de contenidos de la página.

## Reglas de jerarquía

| Regla | Razón |
|-------|-------|
| Un solo `<h1>` por página | Es el título principal; varios confunden el esquema |
| No saltar niveles hacia abajo | De `<h2>` se baja a `<h3>`, nunca directo a `<h4>` |
| El nivel indica jerarquía, no tamaño | El tamaño se controla con CSS, no eligiendo otra etiqueta |
| Se puede subir varios niveles | Tras un `<h4>` puede venir un `<h2>` que abre otra sección mayor |

### Ejemplo correcto vs. incorrecto

```html
<!-- ✅ Jerarquía coherente -->
<h1>Receta de pan</h1>
  <h2>Ingredientes</h2>
  <h2>Preparación</h2>
    <h3>Amasado</h3>

<!-- ❌ Salto de nivel: de h1 a h3 sin h2 -->
<h1>Receta de pan</h1>
  <h3>Ingredientes</h3>
```

## Relación con las secciones

Dentro de elementos como `<section>` o `<article>`, los encabezados siguen contando para el esquema
global. La práctica moderna y robusta es **usar el nivel real** que corresponde a la profundidad en el
documento, no reiniciar a `<h1>` en cada sección (el "outline algorithm" que recalculaba niveles por
sección nunca llegó a implementarse en los navegadores ni en los lectores de pantalla).

## Accesibilidad

> [!info] Navegación por encabezados
> Los lectores de pantalla permiten **saltar de encabezado en encabezado** (tecla `H` en NVDA/JAWS) y
> **listar el esquema completo** de la página. Es una de las formas principales en que un usuario
> ciego se hace una idea de la estructura y va directo a lo que busca. Una jerarquía bien formada es un
> mapa; una rota (niveles saltados, varios `<h1>`, encabezados elegidos por tamaño) desorienta por
> completo.

## Buenas prácticas

> [!tip] Recomendaciones
> - Un único `<h1>` que resuma el contenido de la página (suele coincidir en idea con el `<title>`,
>   aunque sin el nombre del sitio).
> - Elige el nivel por **jerarquía**, ajusta el tamaño con `font-size` en CSS.
> - No dejes huecos: no pases de `<h2>` a `<h4>`.
> - Comprueba el esquema con una extensión de accesibilidad o el árbol de accesibilidad de DevTools.

## Errores comunes

> [!warning] Elegir el nivel por su apariencia
> El error más extendido: usar `<h4>` "porque se ve del tamaño que quiero". Esto rompe el esquema. Si
> un título de nivel 2 debe verse pequeño, se mantiene `<h2>` y se reduce con CSS:
> ```css
> h2 { font-size: 1.1rem; }
> ```
> La etiqueta es semántica; el tamaño es presentación. Igual de común es **maquetar texto grande no
> titular** (un eslogan) con `<h1>`: si no es un título, no es un encabezado (usa `<p>` y CSS).

## Notas relacionadas

- [[02 Agrupación de Título (hgroup)]] — agrupar un encabezado con su subtítulo sin romper el esquema.
- [[01 HTML Semántico como Base]] — los encabezados como columna vertebral de la accesibilidad.
- [[03 Título del Documento (title)]] — el título de la pestaña, que no debe confundirse con el `<h1>`.
