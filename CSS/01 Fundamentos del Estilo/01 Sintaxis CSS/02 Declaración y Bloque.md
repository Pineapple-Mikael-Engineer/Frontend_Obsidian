---
title: "Declaración y Bloque — Pares propiedad:valor agrupados"
aliases:
  - declaration block
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
order: 2
---

# Declaración y Bloque

> [!definicion]
> Una **declaración** es un único par `propiedad: valor` terminado en punto y coma. Un **bloque de declaraciones** es el conjunto de declaraciones agrupadas entre llaves `{ }` que un selector aplica.

```css
.tarjeta {
  /* ↓ bloque de declaraciones */
  color: #1e1e2e;       /* ← una declaración */
  background: #fff;     /* ← otra declaración */
  padding: 1rem;
}
```

## Anatomía de una declaración

```css
color: crimson;
/* │      │     │
   │      │     └ punto y coma (separa de la siguiente)
   │      └ valor
   └ propiedad (seguida de dos puntos) */
```

| Parte | Detalle |
|-------|---------|
| Propiedad | El nombre de la característica |
| `:` | Separa propiedad de valor |
| Valor | Lo asignado a la propiedad |
| `;` | Cierra la declaración |

## El bloque

El bloque agrupa todas las declaraciones de una regla entre `{` y `}`. Puede tener una o muchas declaraciones, y se aplica **entero** al selector:

```css
p { color: navy; }                          /* bloque de 1 declaración */
p { color: navy; line-height: 1.6; margin: 0; }   /* de 3 */
```

## Errores que rompen el bloque

> [!warning] Un error puede tumbar una declaración (no todo el bloque)
> CSS aísla los errores **por declaración**: una declaración inválida se descarta, pero las demás del bloque siguen aplicándose.
> ```css
> p {
>   color: crimsom;     /* ❌ "crimsom" mal escrito: esta se ignora */
>   font-size: 1.2rem;  /* ✅ esta sí se aplica */
> }
> ```
> La excepción es un **`;` olvidado**, que funde dos declaraciones en una inválida, perdiendo ambas. Por eso el `;` es crítico.

## Dos puntos vs. punto y coma

Un error de principiante es confundirlos:

| Símbolo | Función |
|---------|---------|
| `:` (dos puntos) | Separa **propiedad de valor** (`color: red`) |
| `;` (punto y coma) | **Termina** la declaración (`color: red;`) |

## Formato legible

> [!tip] Una declaración por línea
> Aunque CSS ignora los espacios y saltos de línea, el formato convencional —una declaración por línea, indentadas dentro del bloque— hace el código mucho más legible y fácil de mantener:
> ```css
> /* legible */
> .boton {
>   padding: 0.5rem 1rem;
>   border: none;
>   border-radius: 0.5rem;
> }
> ```

## Notas relacionadas

- [[01 Selector, Propiedad y Valor]] — las piezas de la declaración.
- [[03 Regla Completa]] — selector + bloque ensamblados.
- [[03 Comentarios CSS]] — anotar dentro y fuera del bloque.
