---
title: Variables CSS (Propiedades Personalizadas)
aliases:
  - variables css
  - custom properties
  - propiedades personalizadas
tags:
  - css
  - api/concepto
  - variables
draft: false
---

# Variables CSS (Propiedades Personalizadas)

> [!definicion]
> Las **variables CSS** (formalmente, *custom properties*) son propiedades definidas por el autor que almacenan valores reutilizables. Se declaran con `--nombre-variable` y se consumen con `var(--nombre-variable)`. A diferencia de las variables de Sass, existen en tiempo de ejecución en el navegador, respetan la herencia CSS, y pueden manipularse con JavaScript.

```css
:root {
  --color-primary: #3b82f6;
  --space-4: 1rem;
  --radius-md: 6px;
}

.btn {
  background: var(--color-primary);
  padding: var(--space-4);
  border-radius: var(--radius-md);
}
```

## Por qué son mejores que las variables de Sass

Las variables de Sass (y de otros preprocesadores) se resuelven en tiempo de compilación: una vez compilado el CSS, las variables desaparecen y quedan sus valores fijos. Las variables CSS existen **en el navegador**:

- **Heredan**: una variable definida en `:root` está disponible en toda la página; una definida en `.card` está disponible solo en `.card` y sus descendientes.
- **Se pueden cambiar en tiempo real**: con JavaScript o con media queries, sin recompilar nada.
- **Participan en la cascada**: se pueden sobrescribir en selectores más específicos o más internos del DOM.

```css
/* Sass: compila a valores fijos */
$color: blue;
.btn { background: $color; }   /* → background: blue; */

/* CSS custom property: sigue siendo una variable en el navegador */
:root { --color: blue; }
.btn { background: var(--color); }   /* la variable existe en runtime */

/* Se puede cambiar desde JS */
document.documentElement.style.setProperty('--color', 'red');
/* → todos los .btn cambian sin recompilar */
```

## Casos de uso principales

| Caso | Por qué las variables CSS son la herramienta correcta |
|---|---|
| Design tokens (colores, espaciados, tipografía) | Un cambio en `:root` actualiza todo el sistema |
| Temas dinámicos (modo oscuro) | Se cambia el valor de la variable, no las propiedades de cada elemento |
| Componentes parametrizables | El componente expone variables; el contexto las personaliza |
| Valores calculados (`calc()`) | Las variables se componen con `calc()` para spacing relativo |

## Notas de esta sección

- [[01 Definición y Uso (--var, var())]] — sintaxis de declaración y consumo.
- [[02 Valor por Defecto (fallback)]] — `var(--x, valor-por-defecto)`.
- [[03 Ámbito y Herencia de Variables]] — dónde se declaran y cómo se heredan.
- [[04 Variables en Media Queries]] — cambiar valores con `@media`, no los selectores.
- [[05 Manipulación con JavaScript]] — leer y escribir variables desde JS.
- [[06 Temas Dinámicos]] — implementar modo oscuro y temas con variables.

## Notas relacionadas

- [[09 Arquitectura y Metodologías/index]] — las variables como design tokens del sistema de diseño.
- [[05 Tipografía Responsiva/02 clamp() para Tipografía Fluida]] — `clamp()` combinado con variables.
