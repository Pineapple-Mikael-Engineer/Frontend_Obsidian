---
title: OOCSS — Object-Oriented CSS
aliases:
  - OOCSS
  - object-oriented css
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
order: 3
---

# OOCSS — Object-Oriented CSS

> [!definicion]
> **OOCSS** (Object-Oriented CSS) es una metodología creada por Nicole Sullivan que aplica los principios de la programación orientada a objetos al CSS. Sus dos principios fundamentales son: **separar estructura de apariencia** (un objeto visual tiene forma y aspecto como responsabilidades distintas) y **separar contenedor de contenido** (los estilos de un componente no deben depender de dónde está en el DOM).

```css
/* OOCSS — separación de estructura y apariencia */
/* Estructura: define forma y posición */
.media { display: flex; align-items: flex-start; gap: 1rem; }
.media__figura { flex-shrink: 0; }
.media__cuerpo { flex: 1; }

/* Apariencia: define color, tipografía, decoración */
.card-skin { background: white; border: 1px solid #e5e7eb; border-radius: 8px; padding: 1rem; }
.card-skin--sombra { box-shadow: 0 4px 12px rgb(0 0 0 / 0.08); }
```

```html
<!-- Composición: la misma estructura con distintas apariencias -->
<div class="media card-skin">
  <img class="media__figura" src="..." alt="">
  <div class="media__cuerpo">Contenido</div>
</div>

<div class="media card-skin card-skin--sombra">
  <img class="media__figura" src="..." alt="">
  <div class="media__cuerpo">Contenido destacado</div>
</div>
```

## Los dos principios de OOCSS

### Principio 1 — Separar estructura de apariencia

Un "objeto visual" se compone de dos capas independientes:

- **Estructura**: `width`, `height`, `margin`, `padding`, `display`, `position`, `overflow` — todo lo que define la geometría.
- **Apariencia**: `color`, `background`, `border`, `font`, `box-shadow`, `opacity` — todo lo que define el aspecto visual.

Al separarlas en clases distintas, la estructura puede reutilizarse con diferentes apariencias, y la apariencia puede aplicarse a diferentes estructuras.

```css
/* Estructura del botón */
.btn { display: inline-flex; align-items: center; padding: 0.5rem 1rem; border-radius: 4px; cursor: pointer; }

/* Apariencias intercambiables */
.btn-primario { background: #3b82f6; color: white; border: none; }
.btn-secundario { background: transparent; color: #3b82f6; border: 1px solid #3b82f6; }
.btn-peligro { background: #ef4444; color: white; border: none; }
```

```html
<button class="btn btn-primario">Guardar</button>
<button class="btn btn-secundario">Cancelar</button>
<button class="btn btn-peligro">Eliminar</button>
```

### Principio 2 — Separar contenedor de contenido

Los estilos de un componente no deben depender de su contenedor en el DOM. Evita selectores como `.sidebar h2` o `#nav .link`; en su lugar, usa clases directas sobre el elemento.

```css
/* MAL — el estilo del h2 depende de estar en .sidebar */
.sidebar h2 { font-size: 1rem; color: #374151; }
.main-content h2 { font-size: 1.5rem; color: #111; }

/* BIEN — clases independientes del contenedor */
.heading-sidebar { font-size: 1rem; color: #374151; }
.heading-principal { font-size: 1.5rem; color: #111; }
```

Con este principio, el componente es portable: funciona igual si está en la barra lateral, en el contenido principal o en un modal.

## El "Media Object" — el patrón OOCSS canónico

El **media object** (imagen + texto al lado) es el patrón que Nicole Sullivan usó para introducir OOCSS. Es el "hello world" de la metodología:

```css
.media { display: flex; align-items: flex-start; }
.media__figura { flex-shrink: 0; margin-right: 1rem; }
.media__figura--derecha { order: 1; margin-right: 0; margin-left: 1rem; }
.media__cuerpo { flex: 1; }
```

```html
<!-- Imagen a la izquierda -->
<div class="media">
  <img class="media__figura" src="avatar.jpg" width="48" height="48" alt="">
  <div class="media__cuerpo">
    <strong>Usuario</strong>
    <p>Comentario o descripción...</p>
  </div>
</div>

<!-- Imagen a la derecha -->
<div class="media">
  <div class="media__cuerpo">Texto primero</div>
  <img class="media__figura media__figura--derecha" src="...">
</div>
```

## OOCSS vs BEM

OOCSS y BEM resuelven el mismo problema de formas distintas:

| Aspecto | OOCSS | BEM |
|---|---|---|
| Unidad principal | "Objeto visual" (estructura + apariencia por separado) | Bloque (autocontenido) |
| Composición | Muchas clases pequeñas que se combinan | Una clase base + modificadores |
| HTML | Más clases por elemento | Más nombres de clase largos |
| Reutilización | Alta — la estructura existe sola | Media — el bloque incluye su apariencia |
| Legibilidad del HTML | Difícil de seguir con muchas clases | Clara — el nombre del bloque describe todo |

En la práctica, OOCSS lleva a HTML con más clases que BEM, lo que puede ser difícil de mantener en equipos grandes. BEM resolvió esa legibilidad con nombres descriptivos. La influencia de OOCSS se mantiene más en la separación de estructura/apariencia que en el uso literal de sus convenciones de nombres.

## OOCSS como influencia en Utility-First

La lógica de "muchas clases pequeñas y componibles" de OOCSS es el antecedente directo de Tailwind CSS y otros frameworks de utilidades. Utility-First lleva la separación al extremo: una clase por propiedad CSS. OOCSS propone "objetos" reutilizables; Utility-First propone átomos de una sola propiedad.

## Cuándo tiene sentido OOCSS

OOCSS encaja bien cuando el diseño tiene muchos **objetos visuales recombinables**: los mismos colores de fondo con distintos layouts, las mismas tipografías en distintos contenedores. No encaja bien en sistemas de componentes donde cada componente es cerrado y autocontenido (ahí BEM o los módulos CSS son más legibles).

## Buenas prácticas

> [!tip] Recomendaciones
> - Aplica el principio de separar contenedor de contenido siempre: evita `.padre .hijo`; usa `.hijo-en-contexto`.
> - Define las estructuras como clases independientes: `.media`, `.bloque-de-texto`, `.grupo-de-formulario`.
> - Combina con BEM para los componentes más complejos: BEM para el interior del componente, OOCSS para la composición de objetos mayores.
> - Documenta los "objetos" disponibles: sin documentación, el equipo crea duplicados.

## Errores comunes

> [!warning] Trampas
> - **HTML con demasiadas clases**: OOCSS sin disciplina lleva a elementos con 8-10 clases, difíciles de auditar.
> - **Mezclar estructura y apariencia en la misma clase**: derrotas el principio 1.
> - **Selectores anidados**: `.sidebar .btn` rompe el principio 2.
> - **Confundir OOCSS con herencia de OOP**: los "objetos" CSS no tienen herencia ni polimorfismo en el sentido programático.

## Notas relacionadas

- [[index]] — las metodologías en contexto.
- [[01 BEM]] — la alternativa más estructurada para componentes.
- [[04 Utility-First]] — la evolución de la idea de clases pequeñas y componibles.
- [[05 Organización de Archivos/index]] — cómo organizar los "objetos" en archivos.
