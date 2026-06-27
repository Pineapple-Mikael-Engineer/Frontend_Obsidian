---
title: "@layer — Capas de Cascada CSS"
aliases:
  - "@layer"
  - capas css
  - cascade layers
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
order: 3
---

# @layer — Capas de Cascada CSS

> [!definicion]
> `@layer` (cascade layers) permite al autor dividir sus estilos en **capas con prioridad explícita**. Las capas declaradas **después** tienen más prioridad que las anteriores (para estilos normales). Esto resuelve el problema de la escalada de especificidad y hace el cascade predecible sin necesidad de `!important`.

```css
/* Declarar el orden de las capas (las últimas ganan) */
@layer reset, base, components, utilities;

@layer base {
  .btn { color: blue; }
}

@layer utilities {
  .text-red { color: red; }   /* gana a .btn aunque tengan la misma especificidad */
}
```

## El problema que resuelve

Antes de `@layer`, el único modo de garantizar que un estilo ganaba a otro era tener mayor especificidad o aparecer después. Esto llevaba a:
- Selectores cada vez más específicos (ID-hell).
- Dependencia frágil en el orden de los archivos.
- Abuso de `!important` para "forzar" overrides.

Con `@layer`, el orden de prioridad se declara **explícitamente** al inicio del CSS, separando la preocupación de "quién gana" de la especificidad de los selectores.

## Regla de prioridad

**Para estilos normales**: las capas definidas **después** tienen más prioridad.
**Para `!important`**: se invierte — los `!important` de capas **anteriores** tienen más prioridad.
**Estilos sin capa**: tienen más prioridad que cualquier capa (son implícitamente "la última capa").

```css
@layer A, B;

@layer A { .x { color: red; } }
@layer B { .x { color: blue; } }   /* gana — B se declaró después */
.x { color: green; }                /* gana sobre A y B — sin capa */
```

## Casos de uso

| Caso | Cómo usar @layer |
|---|---|
| Resets | Primera capa — la más fácil de sobrescribir |
| Estilos base / design tokens | Segunda capa |
| Componentes de biblioteca tercera | Capa propia para aislarla |
| Componentes de la app | Capa de componentes |
| Utilidades finales | Última capa o sin capa |

## Notas relacionadas

- [[01 Definición y Orden de Capas]] — cómo declarar y apilar capas.
- [[02 Importar a Capas]] — `@import` con layer, aislar terceros.
- [[02 Cascada/index]] — @layer en el contexto de la cascada completa.
- [[02 Importancia (!important)]] — cómo !important interactúa con las capas.
