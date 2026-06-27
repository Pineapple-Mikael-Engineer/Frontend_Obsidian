---
title: "Definición y Uso de Variables CSS (--var, var())"
aliases:
  - "--variable"
  - "var()"
  - custom properties css
tags:
  - css
  - api/concepto
  - variables
propiedad: "--*"
grupo: concepto
draft: false
order: 1
---

# Definición y Uso — --var y var()

> [!definicion]
> Las propiedades personalizadas se **declaran** con el prefijo `--` seguido del nombre (sin espacios), y se **consumen** con la función `var()`. El nombre distingue mayúsculas de minúsculas: `--color` y `--Color` son variables distintas. Se pueden declarar en cualquier selector; el ámbito es el árbol DOM a partir de ese elemento.

```css
/* Declaración */
:root {
  --color-primary: #3b82f6;
  --font-size-base: 1rem;
  --space-4: 1rem;
}

/* Consumo */
.btn {
  color: white;
  background: var(--color-primary);
  font-size: var(--font-size-base);
  padding: var(--space-4);
}
```

## Sintaxis de declaración

```css
selector {
  --nombre-de-variable: valor;
}
```

- El nombre debe comenzar con `--` (dos guiones).
- El resto puede ser cualquier combinación de letras, números, guiones y guiones bajos.
- El valor puede ser cualquier cosa válida en CSS: un color, una longitud, una función, una cadena de texto, un valor numérico sin unidad, incluso valores "en crudo" que se usarán como partes de otras declaraciones.
- Las variables se heredan hacia los descendientes del selector donde se declaran.

```css
/* Ejemplos de declaraciones válidas */
:root {
  --color-texto: #1a1a2e;
  --fuente-heading: "Inter", system-ui, sans-serif;
  --espacio-base: 0.25rem;
  --radio: 6px;
  --sombra: 0 4px 12px rgb(0 0 0 / 0.1);
  --duracion-transicion: 200ms;
  --z-modal: 1000;
  --proporcion: 16/9;   /* valor de ratio para aspect-ratio */
}
```

## Sintaxis de consumo — var()

```css
propiedad: var(--nombre);
propiedad: var(--nombre, valor-por-defecto);
```

`var()` acepta un segundo argumento: el valor por defecto que se usa si la variable no está definida o es inválida (ver nota de fallback).

```css
.btn {
  background: var(--btn-color, #3b82f6);     /* si --btn-color no existe → #3b82f6 */
  border-radius: var(--btn-radius, var(--radius-md, 4px));  /* fallback anidado */
}
```

## Variables en cálculos con calc()

Las variables se componen con `calc()` para crear escalas dinámicas:

```css
:root {
  --espacio-base: 0.25rem;
}

.mt-1 { margin-top: calc(var(--espacio-base) * 1); }   /* 0.25rem */
.mt-2 { margin-top: calc(var(--espacio-base) * 2); }   /* 0.5rem */
.mt-4 { margin-top: calc(var(--espacio-base) * 4); }   /* 1rem */
.mt-8 { margin-top: calc(var(--espacio-base) * 8); }   /* 2rem */
```

Esto establece una escala de espaciado donde cambiar `--espacio-base` reescala toda la aplicación.

## Variables como partes de valores

Las variables pueden ser un fragmento de un valor más grande. El valor de la variable se sustituye como texto antes de que CSS lo interprete:

```css
:root {
  --h: 210;
  --s: 80%;
  --l: 50%;
}

.elemento {
  /* Las variables se sustituyen → hsl(210, 80%, 50%) */
  color: hsl(var(--h), var(--s), var(--l));
  /* Para modo oscuro, solo cambiamos --l */
}
```

Esta técnica es la base de los temas dinámicos: separar los componentes de color (matiz, saturación, luminosidad) en variables independientes.

## Variables sin unidad para usar con calc()

Una variable puede guardar un número sin unidad para usarse dentro de `calc()` con la unidad añadida en el cálculo:

```css
:root {
  --columnas: 3;
  --gap: 1;   /* sin unidad */
}

.grid {
  grid-template-columns: repeat(var(--columnas), 1fr);
  gap: calc(var(--gap) * 1rem);   /* añade la unidad en el calc */
}
```

## Recetas comunes

### Design tokens completos en :root

```css
:root {
  /* Colores */
  --color-primary: #3b82f6;
  --color-primary-dark: #2563eb;
  --color-secondary: #8b5cf6;
  --color-surface: #ffffff;
  --color-on-surface: #1f2937;
  --color-border: #e5e7eb;
  --color-error: #ef4444;

  /* Espaciado (escala de 4px) */
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-4: 1rem;
  --space-6: 1.5rem;
  --space-8: 2rem;
  --space-12: 3rem;
  --space-16: 4rem;

  /* Tipografía */
  --font-sans: system-ui, -apple-system, sans-serif;
  --font-mono: ui-monospace, "Cascadia Code", monospace;
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-lg: 1.125rem;
  --text-xl: 1.25rem;
  --text-2xl: 1.5rem;

  /* Bordes y sombras */
  --radius-sm: 4px;
  --radius-md: 6px;
  --radius-lg: 12px;
  --radius-full: 9999px;
  --shadow-sm: 0 1px 2px rgb(0 0 0 / 0.05);
  --shadow-md: 0 4px 12px rgb(0 0 0 / 0.08);

  /* Transiciones */
  --duration-fast: 150ms;
  --duration-base: 250ms;
  --easing-default: cubic-bezier(0.4, 0, 0.2, 1);
}
```

### Componente que expone su propia API de variables

```css
/* El componente define variables con valor por defecto */
.card {
  --card-bg: var(--color-surface);
  --card-border: var(--color-border);
  --card-radius: var(--radius-md);
  --card-padding: var(--space-4);

  background: var(--card-bg);
  border: 1px solid var(--card-border);
  border-radius: var(--card-radius);
  padding: var(--card-padding);
}

/* El contexto puede personalizar sin sobrescribir los estilos del componente */
.card--compacta {
  --card-padding: var(--space-2);
}

.seccion-premium .card {
  --card-bg: #1e3a5f;
  --card-border: #2563eb;
}
```

## Cómo funciona por dentro

Las propiedades personalizadas no son propiedades CSS reales: son un mapa de pares clave-valor asociado a cada nodo del DOM. Cuando el navegador encuentra `var(--nombre)`, busca `--nombre` en el elemento actual y, si no lo encuentra, sube por el árbol del DOM hasta encontrarlo (herencia). Si llega a la raíz sin encontrarlo, usa el valor por defecto de `var()` o la propiedad queda inválida en tiempo de computación (*invalid at computed value time*).

## Buenas prácticas

> [!tip] Recomendaciones
> - Declara los design tokens globales en `:root` para que estén disponibles en todo el documento.
> - Usa nombres semánticos: `--color-primary`, no `--azul`. Los colores cambian en temas; el nombre semántico no.
> - Expón una API de variables en los componentes complejos: `--card-bg`, `--btn-radius`. Permite personalización sin sobreescribir propiedades.
> - Agrupa las variables por categoría con comentarios: colores, espaciado, tipografía, etc.
> - Prefiere `--espacio-4: 1rem` sobre `--espacio-16px: 16px`: las unidades relativas son más flexibles.

## Errores comunes

> [!warning] Trampas
> - **Espacios antes del `:` en la declaración**: `--color : blue` (con espacio) es válido pero inconsistente con el resto de CSS.
> - **Olvidar que el nombre es case-sensitive**: `--Color` y `--color` son variables distintas.
> - **Variables no definidas sin fallback**: si `--variable` no existe y no hay fallback, la propiedad queda inválida sin error visible en el DOM.
> - **Guardar valores con unidad para usar en `calc()` sin unidad**: `--espacio: 1rem` no puede multiplicarse directamente; `--espacio: 1` (sin unidad) sí.

## Notas relacionadas

- [[02 Valor por Defecto (fallback)]] — el segundo argumento de `var()`.
- [[03 Ámbito y Herencia de Variables]] — cómo se buscan en el árbol DOM.
- [[06 Temas Dinámicos]] — patrón completo con variables para modo oscuro.
- [[05 Manipulación con JavaScript]] — leer y cambiar variables desde JS.
