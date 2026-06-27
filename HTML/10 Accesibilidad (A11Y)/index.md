---
title: Accesibilidad (A11Y) — Una web usable por todos
aliases:
  - accessibility
  - a11y
tags:
  - html
  - api/concepto
  - a11y
draft: false
order: 10
---

# Accesibilidad (A11Y)

> [!definicion]
> La accesibilidad (abreviada **a11y**: "a", 11 letras, "y") es el conjunto de prácticas que hacen una web usable por **todas** las personas, incluidas las que navegan con lector de pantalla, solo con teclado, con baja visión o con dificultades motoras. En HTML, la mayor parte se consigue con **marcado semántico correcto**, no con esfuerzo extra.

```html
<!-- Accesible por defecto: semántica + label + alt -->
<main>
  <h1>Título</h1>
  <img src="foto.jpg" alt="Descripción de la foto" />
  <label for="email">Correo</label>
  <input type="email" id="email" />
</main>
```

## El principio rector

> [!info] La regla nº 1 de ARIA: no usar ARIA
> El principio fundamental de la accesibilidad web: **usa el elemento HTML nativo correcto** antes que recrear su comportamiento con ARIA. Un `<button>` ya es accesible (foco, teclado, rol); un `<div role="button">` hay que dotarlo a mano de todo eso, y casi siempre queda peor. La semántica nativa es accesible **por defecto**; ARIA solo rellena los huecos que el HTML no cubre.

## Mapa de la sección

- [[01 HTML Semántico como Base]] — por qué la semántica es el 90 % de la accesibilidad.
- [[02 ARIA/index]] — roles, propiedades y estados para lo que el HTML no cubre.
- [[03 Navegación por Teclado/index]] — foco, `tabindex`, skip links, trampas de foco.
- [[04 Textos Alternativos/index]] — `alt`, iconos, contenido para lectores de pantalla.
- [[05 Formularios Accesibles]] — etiquetas, errores y agrupación accesibles.
- [[06 Multimedia Accesible/index]] — subtítulos, transcripciones, audiodescripción.

## Las cuatro dimensiones (POUR)

Las WCAG (Web Content Accessibility Guidelines) organizan la accesibilidad en cuatro principios:

| Principio | Significa |
|-----------|-----------|
| **Perceptible** | La información se puede percibir (alt, subtítulos, contraste) |
| **Operable** | Se puede manejar (teclado, sin trampas, tiempo suficiente) |
| **Comprensible** | Se entiende (lenguaje claro, comportamiento predecible) |
| **Robusto** | Funciona con distintas tecnologías de asistencia |

## No es un extra, es la base

> [!tip] Accesible desde el principio
> La accesibilidad no es una capa que se añade al final: es el resultado natural de usar HTML bien. Una semántica correcta, etiquetas en los formularios, `alt` en las imágenes y un orden de foco lógico cubren la mayor parte de las WCAG **sin coste adicional**. Repararla después es caro; hacerla desde el inicio es casi gratis.

## Notas relacionadas

- [[01 HTML Semántico como Base]] — el punto de partida.
- [[02 Estructura Semántica/index]] — los landmarks que dan accesibilidad por defecto.
