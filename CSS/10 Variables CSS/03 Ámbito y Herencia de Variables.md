---
title: Ámbito y Herencia de Variables CSS
aliases:
  - ámbito variables css
  - scope custom properties
  - herencia variables css
tags:
  - css
  - api/concepto
  - variables
draft: false
---

# Ámbito y Herencia de Variables CSS

> [!definicion]
> Las variables CSS tienen un **ámbito basado en el DOM**: una variable declarada en un selector está disponible en ese elemento y en **todos sus descendientes**. Si se declara en `:root`, está disponible en todo el documento. Esta herencia es la que hace las variables CSS dinámicas: redefinir una variable en un elemento concreto cambia el comportamiento de todos los componentes que vivan dentro de ese elemento.

```css
:root { --color: blue; }         /* disponible en todo el documento */

.dark { --color: white; }        /* dentro de .dark, --color es white */

.card { color: var(--color); }   /* blue fuera de .dark, white dentro */
```

```html
<p class="card">Texto azul</p>         <!-- --color: blue -->
<div class="dark">
  <p class="card">Texto blanco</p>     <!-- --color: white (redefinida en .dark) -->
</div>
```

## Cómo funciona la herencia

Cuando el navegador encuentra `var(--nombre)` en un elemento, busca la variable siguiendo este camino:

1. El elemento actual.
2. El padre directo.
3. El abuelo.
4. … hasta `:root`.
5. Si no la encuentra en ningún ancestro, usa el fallback de `var()` o la declaración queda inválida.

Este proceso ocurre **en tiempo de cómputo**, no en tiempo de análisis del CSS. Significa que el valor de `var(--color)` para `.card` depende de dónde esté `.card` en el DOM en cada momento.

```css
:root { --espacio: 1rem; }
.compacto { --espacio: 0.5rem; }

.elemento { margin-bottom: var(--espacio); }
/* → 1rem fuera de .compacto */
/* → 0.5rem dentro de .compacto */
```

## El ámbito como parámetro de componente

La herencia convierte las variables en una forma de **parametrizar componentes desde el exterior**:

```css
/* El componente usa sus propias variables */
.btn {
  background: var(--btn-bg, var(--color-primary, #3b82f6));
  color: var(--btn-color, white);
  padding: var(--btn-padding, 0.5rem 1rem);
}

/* El contenedor redefine las variables — los .btn dentro cambian */
.hero {
  --btn-bg: white;
  --btn-color: var(--color-primary, #3b82f6);
  --btn-padding: 0.75rem 2rem;
}
```

```html
<header class="hero">
  <!-- Este .btn usa los valores de .hero: fondo blanco, texto azul, más grande -->
  <button class="btn">Empezar</button>
</header>

<!-- Este .btn usa los valores por defecto: fondo azul, texto blanco -->
<button class="btn">Guardar</button>
```

## Variables locales vs. globales

### Variables globales (design tokens)

Se declaran en `:root` y son accesibles en todo el documento. Representan los design tokens del sistema: paleta de colores, escala de espaciado, tipografía, etc.

```css
:root {
  /* Tokens de color */
  --color-primary: #3b82f6;
  --color-secondary: #8b5cf6;
  --color-surface: #ffffff;
  --color-text: #1f2937;

  /* Tokens de espaciado */
  --space-1: 0.25rem;
  --space-4: 1rem;
  --space-8: 2rem;
}
```

### Variables locales (API del componente)

Se declaran en el propio componente, con valores por defecto que pueden ser tokens globales. Solo afectan al componente y a sus descendientes.

```css
.card {
  /* API local — se pueden redefinir desde fuera */
  --card-bg: var(--color-surface, white);
  --card-padding: var(--space-4, 1rem);
  --card-radius: var(--radius-md, 6px);
  --card-shadow: var(--shadow-sm, 0 1px 2px rgb(0 0 0 / 0.05));

  background: var(--card-bg);
  padding: var(--card-padding);
  border-radius: var(--card-radius);
  box-shadow: var(--card-shadow);
}
```

## Aislar el ámbito: el truco de la variable no heredable

Las variables CSS siempre heredan hacia abajo. No existe un mecanismo nativo para una variable "local" que no se herede. Sin embargo, se puede simular redefiniendo la variable con el valor `initial` (que para variables es el estado de "no definida"):

```css
.componente-aislado {
  /* Redefinir la variable a "no definida" para que los hijos no la vean */
  --variable-local: initial;

  /* En los hijos, var() usará el fallback porque --variable-local está "vacía" */
}
```

Este truco es útil en casos donde un componente no debería recibir las variables de su contenedor.

## Variables y la cascada

Las variables CSS participan plenamente en la cascada. Si dos selectores definen la misma variable para el mismo elemento, gana la de mayor especificidad (igual que con cualquier otra propiedad):

```css
:root { --color: blue; }          /* especificidad (0,0,0) */
.dark { --color: white; }         /* especificidad (0,1,0) — gana si el elemento tiene .dark */
#hero { --color: gold; }          /* especificidad (1,0,0) — gana si el elemento tiene #hero */
```

## Recetas comunes

### Tema de sección con variables

```css
/* Variante de tema aplicada a una sección */
.seccion-oscura {
  --color-surface: #1e293b;
  --color-text: #f1f5f9;
  --color-border: #334155;
  --color-primary: #60a5fa;

  background: var(--color-surface);
  color: var(--color-text);
}

/* Todos los componentes dentro de .seccion-oscura usan las variables redefinidas */
.seccion-oscura .card {
  /* .card usa var(--color-surface) → #1e293b (el de la sección, no el global) */
}
```

### Grid responsivo parametrizable

```css
.grid-auto {
  --columnas: 3;
  --gap: 1rem;
  --min-col: 15rem;

  display: grid;
  grid-template-columns: repeat(
    var(--columnas, auto-fit),
    minmax(var(--min-col, 15rem), 1fr)
  );
  gap: var(--gap, 1rem);
}

/* En móvil, cambiar a una columna desde fuera */
@media (max-width: 600px) {
  .grid-auto { --columnas: 1; }
}
```

## Cómo funciona por dentro

Internamente, el motor CSS almacena las custom properties en una tabla por nodo del DOM. Cuando se computa un `var()`, el motor consulta la tabla del nodo actual; si no encuentra la propiedad, sube al padre y repite. Esta búsqueda es O(profundidad del árbol) en el peor caso, pero los navegadores modernos la optimizan con invalidación incremental: solo recomputan los nodos afectados cuando cambia una variable.

## Buenas prácticas

> [!tip] Recomendaciones
> - Declara los design tokens globales en `:root`.
> - Usa variables locales (en el propio componente) como API: `--btn-bg`, `--card-padding`.
> - Redefine variables en el contexto que necesita un cambio: `.dark { --color-surface: ... }` en vez de sobrescribir propiedades.
> - Documenta las variables que un componente expone como API personalizable, especialmente si es una biblioteca.

## Errores comunes

> [!warning] Trampas
> - **Asumir que las variables son globales**: solo lo son si se declaran en `:root` o en un ancestro común.
> - **Olvidar que un contenedor puede redefinir la variable**: `.tema-rojo { --color-primary: red }` cambia todos los componentes dentro de `.tema-rojo`.
> - **Redefinir variables en un selector muy específico que no domina el componente**: si el selector no es ancestro del componente, la variable no llega.

## Notas relacionadas

- [[01 Definición y Uso (--var, var())]] — la sintaxis de declaración y consumo.
- [[02 Valor por Defecto (fallback)]] — qué pasa cuando la variable no existe en el ámbito.
- [[06 Temas Dinámicos]] — uso práctico del ámbito para temas dinámicos.
- [[04 Variables en Media Queries]] — cómo cambiar variables en breakpoints.
