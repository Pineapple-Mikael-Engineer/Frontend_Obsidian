---
title: "Valor por Defecto de var() — fallback"
aliases:
  - fallback var()
  - valor por defecto variable css
tags:
  - css
  - api/concepto
  - variables
draft: false
---

# Valor por Defecto (fallback)

> [!definicion]
> `var()` acepta un segundo argumento: el **valor por defecto** (fallback) que se usa cuando la variable no está definida o es inválida para la propiedad en la que se usa. El fallback puede ser cualquier valor CSS válido para esa propiedad, incluida otra llamada a `var()`, permitiendo cadenas de fallbacks.

```css
.btn {
  /* Si --btn-color no existe, usa #3b82f6 */
  background: var(--btn-color, #3b82f6);
}
```

## Sintaxis

```css
var(--nombre)                            /* sin fallback */
var(--nombre, valor-por-defecto)         /* con fallback */
var(--nombre, var(--otro, valor-final))  /* fallback anidado */
```

El separador es una coma. Todo lo que viene después de la primera coma es el fallback, por lo que el fallback puede contener comas sin escapar (lo que permite usar funciones como `rgb()`, `hsl()`, y listas como `margin: 0 1rem 0 0`):

```css
.elemento {
  /* El fallback contiene una coma dentro de rgb() — es válido */
  color: var(--color-texto, rgb(30, 30, 46));

  /* El fallback es una lista de 4 valores */
  margin: var(--margen-base, 0 1rem 0 0);
}
```

## Fallbacks anidados

Se pueden encadenar múltiples niveles de fallback:

```css
.card {
  /* 1. Prueba --card-bg
     2. Si no existe, prueba --color-surface
     3. Si no existe, usa white */
  background: var(--card-bg, var(--color-surface, white));

  /* Cadena de tres niveles */
  border-radius: var(--card-radius, var(--radius-md, var(--radius-sm, 4px)));
}
```

## Cuándo usar fallback

### Componentes que exponen variables personalizables

El fallback da un valor por defecto al componente sin necesitar que el consumidor defina la variable. Si el consumidor quiere personalizar, define la variable; si no, el componente usa su propio default.

```css
/* El componente funciona solo, con sus valores por defecto */
.alert {
  --alert-color: var(--color-primary, #3b82f6);
  --alert-bg: var(--color-primary-light, #eff6ff);
  --alert-border: var(--color-primary-border, #bfdbfe);

  color: var(--alert-color);
  background: var(--alert-bg);
  border-left: 4px solid var(--alert-border);
  padding: var(--space-4, 1rem);
  border-radius: var(--radius-md, 6px);
}

/* Variante error: solo redefine las tres variables */
.alert--error {
  --alert-color: var(--color-error, #ef4444);
  --alert-bg: var(--color-error-light, #fef2f2);
  --alert-border: var(--color-error-border, #fecaca);
}
```

### Compatibilidad progresiva

El fallback puede usarse para dar un valor estático de compatibilidad cuando las variables podrían no estar definidas (por ejemplo, si el CSS de tokens no se cargó):

```css
.btn {
  /* Si no existe --color-primary (tokens no cargados), usa el valor hardcoded */
  background: var(--color-primary, #3b82f6);
  padding: var(--space-4, 1rem);
}
```

### Valores intermedios en cadenas de cálculo

```css
.grid {
  /* Si no se define --cols, usa 3; si no existe calc, usa 3 */
  grid-template-columns: repeat(var(--cols, 3), 1fr);
}
```

## Lo que el fallback NO es: diferencia con el valor inicial

El fallback **no es** el valor inicial (initial value) de la propiedad. El valor inicial es el que CSS asigna cuando no hay ninguna declaración. El fallback es el que `var()` usa cuando **la variable en concreto no existe**.

```css
/* --color no está definido → usa blue (el fallback) */
.elemento { color: var(--color, blue); }   /* color: blue */

/* --color está definido como valor inválido para color, ej: "hola" → usa el valor inicial (black en color) */
:root { --color: hola; }
.elemento { color: var(--color); }         /* color: black (valor inicial de color) */
```

Cuando la variable existe pero su valor es inválido para la propiedad, **no se usa el fallback**: se usa el valor inicial o el valor heredado, según la propiedad. Esto es un comportamiento contraintuitivo pero es el definido en el spec.

## Recetas comunes

### Sistema de tokens con fallback de seguridad

```css
/* tokens.css — define el sistema */
:root {
  --color-primary: #3b82f6;
  --space-4: 1rem;
  --radius-md: 6px;
}

/* components/btn.css — usa tokens con fallback hardcodeado */
.btn {
  background: var(--color-primary, #3b82f6);
  padding: calc(var(--space-4, 1rem) * 0.5) var(--space-4, 1rem);
  border-radius: var(--radius-md, 6px);
}
```

El fallback hardcodeado hace que el componente sea autosuficiente: funciona aunque `tokens.css` no se cargue.

### Fallback para gradiente complejo

```css
.hero {
  background: var(
    --hero-bg,
    linear-gradient(135deg, #1e3a5f 0%, #2563eb 100%)
  );
}
```

## Cómo funciona por dentro

Cuando el navegador evalúa `var(--nombre, fallback)`, primero comprueba si `--nombre` existe en el mapa de custom properties del elemento (recorriendo el árbol hacia `:root`). Si no lo encuentra, sustituye `var(--nombre, fallback)` por el texto del fallback y lo reinterpreta. Si el fallback tampoco es válido para la propiedad, la declaración se trata como si no existiera (la propiedad toma su valor inicial o heredado).

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa fallback en componentes de biblioteca que el consumidor usará sin garantía de tener todos tus tokens.
> - El fallback debe ser el valor "razonable por defecto": si el componente es azul por diseño, el fallback es el azul del sistema.
> - Evita fallbacks demasiado profundos (más de 3 niveles): hacen el CSS difícil de razonar.
> - Documenta las variables que el componente expone como API: `--card-bg`, `--card-padding`, etc.

## Errores comunes

> [!warning] Trampas
> - **Esperar que el fallback se use cuando la variable es inválida**: si la variable existe pero su valor es inválido para la propiedad, CSS usa el valor inicial, no el fallback.
> - **Olvidar que el fallback puede contener comas**: no necesitas escapar las comas dentro de funciones CSS en el fallback.
> - **Fallback incorrecto para la propiedad**: `var(--no-existe, hello)` en `color` no usa "hello" (no es un color válido); la propiedad queda en su valor inicial.

## Notas relacionadas

- [[01 Definición y Uso (--var, var())]] — la sintaxis completa de declaración y consumo.
- [[03 Ámbito y Herencia de Variables]] — por qué la variable puede no existir en un contexto concreto.
- [[06 Temas Dinámicos]] — el fallback como puente entre el tema base y los temas alternativos.
