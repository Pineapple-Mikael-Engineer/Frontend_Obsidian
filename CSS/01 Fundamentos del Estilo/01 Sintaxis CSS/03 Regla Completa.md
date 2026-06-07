---
title: Regla Completa — Selector y bloque ensamblados
aliases:
  - css rule
  - ruleset
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# Regla Completa

> [!definicion]
> Una **regla** (ruleset) es la unidad completa: un **selector** (o varios) seguido de un **bloque de declaraciones**. Una hoja de estilos es, esencialmente, una secuencia de reglas que el navegador aplica al documento.

```css
selector {
  propiedad: valor;
  propiedad: valor;
}
```

## Varios selectores: la agrupación

Una misma regla puede aplicarse a **varios selectores** separados por comas, evitando repetir el bloque:

```css
h1, h2, h3 {
  font-family: Georgia, serif;
  color: #1e1e2e;
}
```

Equivale a escribir tres reglas idénticas. El detalle en [[05 Agrupación de Selectores]].

> [!warning] La coma es un "o", y un error la rompe entera
> En `h1, h2, h3 { }`, la regla afecta a `h1` **o** `h2` **o** `h3`. Cuidado: si **uno** de los selectores de la lista es **inválido** (sintaxis no reconocida), los navegadores antiguos descartaban **toda** la regla. Con selectores modernos esto se mitiga con [[02 where()|`:where()`]]/[[01 is()|`:is()`]], pero conviene saberlo.

## Tipos de regla

No todas las reglas son selector + bloque; CSS tiene **at-rules** (reglas-arroba) que empiezan por `@`:

| Tipo | Ejemplo | Para qué |
|------|---------|----------|
| Regla de estilo | `h1 { … }` | Aplicar estilos a elementos |
| `@media` | `@media (min-width: 600px) { … }` | Estilos condicionales (responsive) |
| `@font-face` | `@font-face { … }` | Definir una fuente web |
| `@keyframes` | `@keyframes girar { … }` | Definir una animación |
| `@import` | `@import "base.css";` | Importar otra hoja |

Las at-rules se desarrollan en sus secciones ([[02 Media Queries/index | media queries]], [[01 Fuentes Web (@font-face)/index | @font-face]], [[04 Animaciones (@keyframes)/index | @keyframes]]).

## Una hoja de estilos completa

```css
/* Reset básico */
* { margin: 0; box-sizing: border-box; }

/* Tipografía base */
body {
  font-family: system-ui, sans-serif;
  line-height: 1.6;
  color: #1e1e2e;
}

/* Componente */
.boton {
  padding: 0.5rem 1rem;
  background: #cba6f7;
  border-radius: 0.5rem;
}

/* Responsive */
@media (min-width: 768px) {
  .boton { padding: 0.75rem 1.5rem; }
}
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Agrupa con comas las reglas que comparten estilos, pero no fuerces agrupaciones que dificulten el mantenimiento.
> - Ordena las reglas de forma coherente (reset → base → componentes → utilidades), tema de [[09 Arquitectura y Metodologías/index | arquitectura]].
> - Comenta las secciones de la hoja para orientarte.

## Notas relacionadas

- [[02 Declaración y Bloque]] — las piezas internas de la regla.
- [[05 Agrupación de Selectores]] — varios selectores en una regla.
- [[02 Cascada/index]] — cómo el navegador resuelve reglas que compiten.
