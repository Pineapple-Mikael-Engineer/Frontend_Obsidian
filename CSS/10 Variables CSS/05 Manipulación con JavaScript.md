---
title: Manipulación de Variables CSS con JavaScript
aliases:
  - variables css javascript
  - setProperty css variable
  - getPropertyValue css variable
tags:
  - css
  - api/concepto
  - variables
draft: false
order: 5
---

# Manipulación con JavaScript

> [!definicion]
> Las variables CSS se pueden leer y escribir desde JavaScript usando la API del DOM: `element.style.setProperty('--nombre', valor)` para escribir y `getComputedStyle(element).getPropertyValue('--nombre')` para leer. Esto permite que la lógica JS modifique el diseño a través de variables sin tocar propiedades CSS individuales.

```js
// Leer una variable
const color = getComputedStyle(document.documentElement)
  .getPropertyValue('--color-primary').trim();

// Escribir una variable
document.documentElement.style.setProperty('--color-primary', '#ef4444');

// Eliminar una variable (vuelve al valor heredado)
document.documentElement.style.removeProperty('--color-primary');
```

> [!info] Esta nota delega los detalles de la API de JavaScript al curso de JavaScript
> Los conceptos completos de `getComputedStyle`, `style.setProperty`, `CSSStyleDeclaration`, y el manejo de eventos relacionados se tratan en las notas de JavaScript correspondientes. Aquí se cubre el patrón CSS y el punto de contacto con JS.

## El patrón: JS cambia variables, CSS hace el trabajo visual

La separación de responsabilidades más limpia es:
- **JavaScript** decide qué cambiar y cuándo.
- **CSS** define cómo se ve el resultado.

JS no debería manipular colores ni píxeles directamente: debería cambiar variables semánticas. El CSS sabe cómo usar esas variables para producir el diseño correcto.

```js
/* MAL: JS manipula estilos individuales */
elemento.style.backgroundColor = '#ef4444';
elemento.style.color = 'white';
elemento.style.boxShadow = '0 4px 12px rgb(239 68 68 / 0.4)';

/* BIEN: JS cambia una variable semántica */
elemento.style.setProperty('--estado', 'error');
/* CSS define todo lo que "error" implica */
```

```css
/* CSS define el significado de cada estado */
.card[style*="--estado: error"] {
  background: var(--color-error-light, #fef2f2);
  border-color: var(--color-error, #ef4444);
  color: var(--color-error-dark, #7f1d1d);
}
```

## Casos de uso

### Tema dinámico con toggle

```js
const toggleTema = () => {
  const root = document.documentElement;
  const temaActual = root.getAttribute('data-theme');
  root.setAttribute('data-theme', temaActual === 'dark' ? 'light' : 'dark');
};
```

```css
:root { --color-bg: #fff; --color-text: #111; }
[data-theme="dark"] { --color-bg: #1a1a2e; --color-text: #e2e8f0; }

body { background: var(--color-bg); color: var(--color-text); }
```

### Posición de un elemento controlada desde JS

```js
/* Seguimiento del cursor */
document.addEventListener('mousemove', (e) => {
  document.documentElement.style.setProperty('--cursor-x', `${e.clientX}px`);
  document.documentElement.style.setProperty('--cursor-y', `${e.clientY}px`);
});
```

```css
/* El CSS usa las variables actualizadas por JS para posicionar el spotlight */
.spotlight {
  width: 300px;
  height: 300px;
  border-radius: 50%;
  background: radial-gradient(circle, rgb(255 255 255 / 0.15), transparent 70%);
  position: fixed;
  left: var(--cursor-x, -9999px);
  top: var(--cursor-y, -9999px);
  translate: -50% -50%;
  pointer-events: none;
}
```

### Barra de progreso

```js
const actualizarProgreso = (porcentaje) => {
  document.querySelector('.barra').style.setProperty('--progreso', porcentaje);
};
```

```css
.barra {
  --progreso: 0;
  height: 8px;
  background: var(--color-border, #e5e7eb);
  border-radius: var(--radius-full, 9999px);
  overflow: hidden;
}

.barra::after {
  content: "";
  display: block;
  height: 100%;
  width: calc(var(--progreso) * 1%);
  background: var(--color-primary, #3b82f6);
  transition: width 0.3s ease;
}
```

### Leer variables para lógica en JS

```js
/* Leer el breakpoint de tablet para lógica de JS */
const breakpointMd = getComputedStyle(document.documentElement)
  .getPropertyValue('--breakpoint-md')
  .trim();

const esMobile = window.matchMedia(`(max-width: ${breakpointMd})`).matches;
```

```css
:root { --breakpoint-md: 768px; }
```

Este patrón mantiene los breakpoints en un único lugar (CSS) y los comparte con JS.

## Recetas comunes

### Sistema de tema con preferencia guardada

```js
const aplicarTema = (tema) => {
  document.documentElement.setAttribute('data-theme', tema);
  localStorage.setItem('tema-preferido', tema);
};

const temaGuardado = localStorage.getItem('tema-preferido')
  || (window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light');

aplicarTema(temaGuardado);
```

### Animación controlada por JS vía variables

```js
/* En lugar de animar directamente con JS, se actualiza la variable */
let progreso = 0;
const animar = () => {
  progreso = Math.min(progreso + 0.01, 1);
  document.querySelector('.loader').style.setProperty('--progreso', progreso);
  if (progreso < 1) requestAnimationFrame(animar);
};
requestAnimationFrame(animar);
```

```css
.loader {
  --progreso: 0;
  width: 200px;
  height: 200px;
  border-radius: 50%;
  background: conic-gradient(
    var(--color-primary, #3b82f6) calc(var(--progreso) * 360deg),
    var(--color-border, #e5e7eb) 0deg
  );
}
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `data-theme` en el elemento raíz (`<html>` o `<body>`) para los temas: semántico y consultable desde CSS.
> - Nombra las variables que JS manipula con prefijos que indiquen su naturaleza dinámica: `--js-cursor-x`, `--js-progreso`.
> - Siempre usa `.trim()` al leer variables: `getPropertyValue` puede incluir espacios alrededor del valor.
> - Para cambios globales, usa `document.documentElement`; para cambios locales, el elemento concreto.

## Errores comunes

> [!warning] Trampas
> - **Leer variables con `element.style.getPropertyValue`**: eso solo lee los estilos inline. Para leer variables heredadas (de `:root`), usa `getComputedStyle(element).getPropertyValue('--nombre')`.
> - **Olvidar `.trim()`**: el valor retornado por `getPropertyValue` puede tener espacios: `' #3b82f6'` en vez de `'#3b82f6'`.
> - **Manipular estilos individuales desde JS** cuando una variable semántica sería más limpio.

## Notas relacionadas

- [[01 Definición y Uso (--var, var())]] — la sintaxis de las variables en CSS.
- [[06 Temas Dinámicos]] — el patrón completo de temas con JS + CSS.
- [[03 Ámbito y Herencia de Variables]] — por qué cambiar `--variable` en `:root` afecta a todo el documento.
