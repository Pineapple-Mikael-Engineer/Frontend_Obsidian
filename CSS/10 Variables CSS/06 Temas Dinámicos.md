---
title: Temas Dinámicos con Variables CSS
aliases:
  - temas dinámicos css
  - dark mode css variables
  - modo oscuro css
tags:
  - css
  - api/concepto
  - variables
draft: false
order: 6
---

# Temas Dinámicos

> [!definicion]
> Los **temas dinámicos** son variantes de apariencia completa (modo oscuro, paletas alternativas, temas de marca) implementadas redefiniendo un conjunto de variables CSS en el elemento raíz. El cambio de tema es instantáneo: los componentes no necesitan saber que el tema existe; solo usan variables semánticas que el tema redefine.

```css
/* Tema claro (por defecto) */
:root {
  --color-bg: #ffffff;
  --color-surface: #f9fafb;
  --color-text: #111827;
  --color-text-muted: #6b7280;
  --color-border: #e5e7eb;
  --color-primary: #3b82f6;
}

/* Tema oscuro */
[data-theme="dark"] {
  --color-bg: #0f172a;
  --color-surface: #1e293b;
  --color-text: #f1f5f9;
  --color-text-muted: #94a3b8;
  --color-border: #334155;
  --color-primary: #60a5fa;
}

/* Los componentes solo usan variables — funcionan en cualquier tema */
body { background: var(--color-bg); color: var(--color-text); }
.card { background: var(--color-surface); border: 1px solid var(--color-border); }
```

## La arquitectura de un sistema de temas

Un sistema de temas bien construido tiene tres capas:

**1. Variables semánticas** — el contrato entre el diseño y los componentes. Los componentes solo deben usar estas variables, nunca valores hardcodeados.

```css
:root {
  /* Semánticas — lo que usan los componentes */
  --color-bg: ;                    /* fondo de la página */
  --color-surface: ;               /* fondo de tarjetas y elementos elevados */
  --color-text: ;                  /* texto principal */
  --color-text-muted: ;            /* texto secundario / atenuado */
  --color-border: ;                /* bordes */
  --color-primary: ;               /* acción principal */
  --color-primary-hover: ;         /* estado hover de la acción principal */
  --color-error: ;                 /* estados de error */
  --color-success: ;               /* estados de éxito */
}
```

**2. Valores por tema** — cada tema asigna valores concretos a las semánticas.

```css
:root, [data-theme="light"] {
  --color-bg: #ffffff;
  --color-surface: #f9fafb;
  --color-text: #111827;
  --color-text-muted: #6b7280;
  --color-border: #e5e7eb;
  --color-primary: #3b82f6;
  --color-primary-hover: #2563eb;
  --color-error: #ef4444;
  --color-success: #22c55e;
}

[data-theme="dark"] {
  --color-bg: #0f172a;
  --color-surface: #1e293b;
  --color-text: #f1f5f9;
  --color-text-muted: #94a3b8;
  --color-border: #334155;
  --color-primary: #60a5fa;
  --color-primary-hover: #93c5fd;
  --color-error: #f87171;
  --color-success: #4ade80;
}
```

**3. Componentes** — solo usan las variables semánticas; son agnósticos al tema.

```css
.btn-primario {
  background: var(--color-primary);
  color: white;
}
.btn-primario:hover { background: var(--color-primary-hover); }

.card {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  color: var(--color-text);
}
```

## Respetar la preferencia del sistema

```css
/* Tema por defecto: claro */
:root {
  --color-bg: #ffffff;
  --color-text: #111827;
  /* ... */
}

/* Respetar preferencia del sistema operativo */
@media (prefers-color-scheme: dark) {
  :root {
    --color-bg: #0f172a;
    --color-text: #f1f5f9;
    /* ... */
  }
}

/* Sobrescribir con tema manual (el usuario elige explícitamente) */
[data-theme="light"] {
  --color-bg: #ffffff;
  --color-text: #111827;
}

[data-theme="dark"] {
  --color-bg: #0f172a;
  --color-text: #f1f5f9;
}
```

Este patrón usa la preferencia del sistema como base y permite que el usuario la sobrescriba con el selector `data-theme`.

## Toggle de tema con JavaScript

```js
const TEMA_KEY = 'tema-preferido';

const obtenerTemaInicial = () => {
  const guardado = localStorage.getItem(TEMA_KEY);
  if (guardado) return guardado;
  return window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
};

const aplicarTema = (tema) => {
  document.documentElement.setAttribute('data-theme', tema);
  localStorage.setItem(TEMA_KEY, tema);
};

// Al cargar la página
aplicarTema(obtenerTemaInicial());

// Al pulsar el toggle
document.querySelector('.btn-tema').addEventListener('click', () => {
  const actual = document.documentElement.getAttribute('data-theme');
  aplicarTema(actual === 'dark' ? 'light' : 'dark');
});
```

## Temas de color basados en HSL

Separar los componentes de color (hue, saturation, lightness) en variables independientes permite crear temas de color que mantienen la estructura de luminosidad pero cambian el matiz:

```css
:root {
  /* Componentes de color — el matiz define el "color de marca" */
  --hue: 220;                         /* azul */
  --sat: 80%;

  /* Los colores derivados mantienen la escala de luminosidad */
  --color-primary: hsl(var(--hue), var(--sat), 50%);
  --color-primary-dark: hsl(var(--hue), var(--sat), 40%);
  --color-primary-light: hsl(var(--hue), var(--sat), 95%);
}

/* Para cambiar el "color de marca" de toda la aplicación, solo cambiar --hue */
[data-brand="purple"] { --hue: 270; }
[data-brand="green"] { --hue: 145; --sat: 65%; }
[data-brand="red"] { --hue: 0; }
```

## Temas de componente (scope reducido)

No todos los temas afectan a toda la página. Un componente puede tener variantes temáticas en ámbito local:

```css
.alerta {
  --alerta-bg: var(--color-surface, #f9fafb);
  --alerta-border: var(--color-border, #e5e7eb);
  --alerta-icon: "ℹ️";

  background: var(--alerta-bg);
  border-left: 4px solid var(--alerta-border);
  padding: var(--space-4, 1rem);
}

.alerta--exito {
  --alerta-bg: #f0fdf4;
  --alerta-border: #22c55e;
  --alerta-icon: "✓";
}

.alerta--error {
  --alerta-bg: #fef2f2;
  --alerta-border: #ef4444;
  --alerta-icon: "✕";
}
```

## Recetas comunes

### Sistema completo de design tokens con temas

```css
/* Paso 1: primitivos (valores concretos, no se usan en componentes) */
:root {
  --azul-50: #eff6ff;
  --azul-500: #3b82f6;
  --azul-600: #2563eb;
  --gris-50: #f9fafb;
  --gris-100: #f3f4f6;
  --gris-900: #111827;
  /* ... */
}

/* Paso 2: semánticos (los componentes solo usan estos) */
:root, [data-theme="light"] {
  --color-bg: var(--gris-50);
  --color-surface: white;
  --color-primary: var(--azul-500);
  --color-primary-hover: var(--azul-600);
  --color-text: var(--gris-900);
}

[data-theme="dark"] {
  --color-bg: #0f172a;
  --color-surface: #1e293b;
  --color-primary: var(--azul-400, #60a5fa);
  --color-primary-hover: var(--azul-300, #93c5fd);
  --color-text: #f1f5f9;
}
```

### Tema personalizable por el usuario (color picker)

```js
document.querySelector('#color-picker').addEventListener('input', (e) => {
  document.documentElement.style.setProperty('--color-primary', e.target.value);
  /* Derivar colores secundarios si es necesario */
});
```

## Cómo funciona por dentro

Cuando se cambia el atributo `data-theme`, el navegador invalida las custom properties de todos los elementos afectados por el nuevo selector `[data-theme="dark"]`. La recalculación es eficiente: solo se recomputan las propiedades que dependen de las variables cambiadas. Con `data-theme` en `<html>`, casi todo el documento se invalida, pero los navegadores modernos procesan el cambio en una sola pasada de estilo, lo que hace que el cambio de tema sea instantáneo incluso en documentos grandes.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `data-theme` en `<html>` para que los temas apliquen a todo, incluidos los elementos de formulario del navegador.
> - Respeta siempre `prefers-color-scheme` como valor por defecto, antes de que el usuario elija explícitamente.
> - Guarda la preferencia del usuario en `localStorage` y aplícala antes de que la página renderice (en `<head>`) para evitar el "flash" de tema incorrecto.
> - Nunca hardcodees colores en los componentes: usa solo variables semánticas (`--color-surface`, no `#ffffff`).
> - Verifica el contraste de color en **ambos temas** con herramientas de accesibilidad.

## Errores comunes

> [!warning] Trampas
> - **Flash de tema incorrecto (FOIT)**: aplicar el tema después de que el DOM cargue produce un destello. La solución es aplicarlo en un `<script>` síncrono en `<head>`.
> - **Usar `prefers-color-scheme` sin permitir override manual**: el usuario puede preferir modo oscuro en el SO pero claro en tu app.
> - **Olvidar las imágenes**: las imágenes hardcodeadas no cambian con el tema. Usa fondos CSS o el atributo `srcset` con variantes para distintos esquemas de color.
> - **Contraste insuficiente en modo oscuro**: los colores que funcionan en claro a menudo no cumplen los ratios de contraste en oscuro.

## Notas relacionadas

- [[01 Definición y Uso (--var, var())]] — la sintaxis de variables que hace posibles los temas.
- [[03 Ámbito y Herencia de Variables]] — por qué `data-theme` en el raíz afecta a todo el árbol.
- [[05 Manipulación con JavaScript]] — el toggle de tema desde JS.
- [[04 Variables en Media Queries]] — `prefers-color-scheme` para el tema automático del SO.
- [[06 Preferencias (prefers-color-scheme, prefers-reduced-motion)]] — la media query de preferencia de color.
