---
title: Breakpoints Comunes — Dónde poner los cortes
aliases:
  - breakpoints
tags:
  - css
  - api/concepto
  - responsive
draft: false
---

# Breakpoints Comunes

> [!definicion]
> Un **breakpoint** es el ancho en el que el layout **cambia** mediante una media query. No hay valores "oficiales"; los habituales se inspiran en familias de dispositivos, pero la mejor práctica es ponerlos **donde el diseño lo necesite**, no donde "está el iPhone".

```css
@media (min-width: 640px)  { /* móvil grande / tablet pequeña */ }
@media (min-width: 768px)  { /* tablet */ }
@media (min-width: 1024px) { /* escritorio */ }
@media (min-width: 1280px) { /* escritorio grande */ }
```

## Los breakpoints típicos

| Breakpoint | Ancho aprox. | Dispositivo de referencia |
|------------|--------------|----------------------------|
| `sm` | ~640px | Móvil grande / tablet pequeña |
| `md` | ~768px | Tablet |
| `lg` | ~1024px | Escritorio / portátil |
| `xl` | ~1280px | Escritorio grande |
| `2xl` | ~1536px | Monitor grande |

Estos valores (los de Tailwind, muy extendidos) son una referencia razonable, pero no una regla.

## La mejor práctica: breakpoints según el contenido

> [!tip] Pon los cortes donde el diseño se rompe
> El consejo más importante: **no** pongas breakpoints en "el ancho del iPhone" o "del iPad" (hay cientos de tamaños de dispositivo y cambian cada año). Pon un breakpoint **donde tu diseño deja de verse bien**: ensancha la ventana del navegador poco a poco y, cuando el layout se rompa (el texto se estira de más, las columnas se aprietan), ahí va el corte. Así los breakpoints se basan en **tu contenido**, no en dispositivos concretos.

## Menos breakpoints con CSS fluido

> [!info] El CSS moderno reduce la necesidad de breakpoints
> Muchos ajustes que antes requerían breakpoints hoy se resuelven sin ninguno:
> - Tipografía y espaciado fluidos con [[07 min(), max(), clamp() | `clamp()`]].
> - Rejillas que se reorganizan solas con [[12 Patrones (auto-fit, auto-fill) | `auto-fit`]].
> - Componentes que se adaptan a su contenedor con [[07 Container Queries/index | container queries]].
>
> Los breakpoints quedan para los **cambios estructurales** grandes (menú a hamburguesa, barra lateral que aparece). Un diseño fluido bien hecho necesita pocos.

## Definir los breakpoints en variables (con cuidado)

> [!warning] Las media queries no pueden usar var() directamente
> Una limitación que sorprende: **las custom properties (`var()`) no funcionan dentro de la condición de una `@media`**:
> ```css
> @media (min-width: var(--bp-md)) { }   /* ❌ no funciona */
> ```
> El ancho de la media query debe ser un valor literal. Para "variables de breakpoint" se usan preprocesadores (Sass) o, modernamente, las `@custom-media` (aún en propuesta). Es una de las pocas cosas donde las variables CSS no llegan.

## Buenas prácticas

> [!tip] Recomendaciones
> - Pon los breakpoints **donde tu diseño se rompe**, no en anchos de dispositivos concretos.
> - Usa CSS fluido (`clamp`, `auto-fit`) para reducir el número de breakpoints.
> - Reserva las media queries para cambios estructurales grandes.
> - Los valores típicos (640/768/1024/1280) son una referencia, no una regla.

## Errores comunes

> [!warning] Trampas
> - **Breakpoints por dispositivo** ("el iPhone mide X"): hay infinitos tamaños.
> - **Demasiados breakpoints**: señal de que falta fluidez en el diseño.
> - **Esperar `var()` en `@media`**: no funciona; usa un preprocesador.

## Notas relacionadas

- [[01 Sintaxis @media]] — cómo escribir las media queries.
- [[07 min(), max(), clamp()]] — fluidez que reduce breakpoints.
- [[01 Mobile First]] — el enfoque que ordena los breakpoints.
