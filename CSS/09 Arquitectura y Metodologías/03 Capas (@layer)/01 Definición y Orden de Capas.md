---
title: "Definición y Orden de Capas (@layer)"
aliases:
  - "definir @layer"
  - orden de capas css
tags:
  - css
  - api/concepto
  - arquitectura
propiedad: "@layer"
grupo: at-rule
draft: false
---

# Definición y Orden de Capas

> [!definicion]
> Las capas de cascada se definen con `@layer`. Se pueden declarar todas a la vez al inicio (estableciendo el orden de prioridad) y luego poblar cada capa por separado. El orden de prioridad queda fijado por la primera declaración de cada capa, no por el orden de los bloques que la pueblan.

```css
/* 1. Declarar el orden (las últimas tienen más prioridad) */
@layer reset, base, components, utilities;

/* 2. Poblar cada capa en cualquier orden posterior */
@layer components {
  .btn { color: blue; padding: 0.5rem 1rem; }
}

@layer base {
  * { box-sizing: border-box; }
  body { font-family: system-ui; }
}
```

## Sintaxis de declaración

### Declaración de orden (sin bloque)

```css
/* Forma recomendada: una sola línea al inicio del CSS principal */
@layer reset, base, layout, components, utilities;
```

Esta forma registra los nombres de las capas y establece su prioridad en el orden en que aparecen (las últimas ganan). No define estilos: solo fija la jerarquía.

### Bloque de capa

```css
/* Asignar estilos a una capa */
@layer components {
  .card { border: 1px solid #e5e7eb; border-radius: 8px; padding: 1rem; }
  .card-title { font-size: 1.25rem; font-weight: 600; }
}
```

El bloque puede aparecer varias veces; los estilos se acumulan en la misma capa. El orden de aparición de los bloques no cambia la prioridad de la capa — esta quedó fijada en la declaración inicial.

### Capa anónima

```css
/* Sin nombre — no puede acumularse después; es una capa de un solo uso */
@layer {
  .util { display: flex; }
}
```

Las capas anónimas son útiles para encapsular un bloque sin necesidad de nombre, pero no se pueden referenciar después.

## Regla fundamental de prioridad

Para estilos normales, **la última capa declarada gana**:

```css
@layer A, B, C;   /* C > B > A */

@layer A { .x { color: red; } }
@layer B { .x { color: blue; } }
@layer C { .x { color: green; } }   /* gana */
```

La especificidad solo importa dentro de la misma capa. Entre capas distintas, la capa de mayor prioridad gana siempre, incluso con selectores menos específicos.

```css
@layer base, components;

@layer base {
  #hero.especial { color: red; }   /* (1,1,0) — muy específico */
}

@layer components {
  .btn { color: blue; }            /* (0,1,0) — menos específico pero capa posterior → GANA */
}
```

## Estilos sin capa (unlayered)

Los estilos fuera de cualquier `@layer` se tratan como si estuvieran en una capa implícita posterior a todas las capas declaradas. Siempre ganan sobre los estilos en capas.

```css
@layer base, components;

@layer components {
  .btn { color: blue; }
}

/* Sin capa — gana sobre components aunque tenga igual especificidad */
.btn { color: red; }
```

Esta regla es importante al integrar bibliotecas de terceros: si su CSS está fuera de capas, gana sobre tus capas. La solución es importarlo dentro de una capa (ver nota de importar a capas).

## Capas anidadas

Las capas pueden anidarse con notación de punto:

```css
@layer framework.base, framework.components;

@layer framework.components {
  .btn { color: blue; }
}
```

Las capas anidadas siguen la misma regla de prioridad: dentro de `framework`, `components` gana sobre `base`. La capa padre `framework` compite como un todo con otras capas de primer nivel.

## Recetas comunes

### Arquitectura de cuatro capas para una app

```css
/* main.css */
@layer reset, tokens, components, utilities;

@layer reset {
  *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }
  body { line-height: 1.5; -webkit-font-smoothing: antialiased; }
}

@layer tokens {
  :root {
    --color-primary: #3b82f6;
    --space-4: 1rem;
    --radius-md: 6px;
  }
}

@layer components {
  .btn {
    background: var(--color-primary);
    padding: var(--space-4);
    border-radius: var(--radius-md);
  }
}

@layer utilities {
  .flex { display: flex; }
  .hidden { display: none; }
  .text-center { text-align: center; }
}
```

### Aislar una biblioteca de terceros

```css
@layer reset, third-party, base, components;

@layer third-party {
  /* Los estilos de la biblioteca van aquí — baja prioridad */
}
```

La biblioteca ocupa `third-party`, que está antes que `base` y `components`. Tus estilos de componente ganan sin `!important`.

## Cómo funciona por dentro

El motor CSS procesa todas las declaraciones `@layer` sin bloque al inicio para registrar el orden de prioridad. Luego, al poblar cada capa con bloques, asigna a cada declaración un par `(índice_de_capa, posición_de_fuente)`. El algoritmo de cascada usa ese índice como el segundo factor de comparación (después de origen/importancia), antes de especificidad y orden de fuente. Las capas anónimas reciben un índice autoincremental cada vez que aparece el bloque.

## Buenas prácticas

> [!tip] Recomendaciones
> - Declara siempre el orden de capas al inicio del CSS principal: `@layer reset, base, components, utilities;`
> - Nombra las capas con sustantivos descriptivos de su función (no del framework).
> - Agrupa las reglas de una capa en un bloque; es más legible que dispersarlas.
> - No anidar capas más de un nivel: aumenta la complejidad sin beneficio proporcional.
> - Importa CSS de terceros siempre dentro de una `@layer` para controlar su prioridad.

## Errores comunes

> [!warning] Trampas
> - **Confundir "última capa declarada" con "último bloque @layer"**: el orden lo fija la declaración sin bloque, no los bloques.
> - **Olvidar que estilos sin capa ganan sobre todos los de capas**: si tienes CSS suelto fuera de capas, tendrá máxima prioridad.
> - **Asumir que más especificidad dentro de una capa gana a otra capa**: la capa de mayor rango siempre gana, sin importar la especificidad.

## Notas relacionadas

- [[index]] — @layer en el contexto arquitectural.
- [[02 Importar a Capas]] — `@import layer()`, aislar terceros.
- [[02 Cascada/index]] — @layer como segundo factor de la cascada.
- [[02 Importancia (!important)]] — !important invierte la prioridad de capas.
