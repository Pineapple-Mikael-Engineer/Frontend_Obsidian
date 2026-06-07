---
title: Selector Universal — Todos los elementos (*)
aliases:
  - universal selector
tags:
  - css
  - api/selector
  - arquitectura
selector: universal
draft: false
---

# Selector Universal

> [!definicion]
> El selector **universal** `*` apunta a **todos** los elementos del documento. Tiene especificidad **cero**, lo que lo hace ideal para estilos por defecto que cualquier otro selector puede sobrescribir. Su uso más común es el *reset* de propiedades base.

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
```

## El uso estrella: el reset de box-sizing

> [!tip] El reset moderno más común
> Casi todos los proyectos empiezan con una versión de este reset, que hace que `width`/`height` incluyan padding y borde en **todos** los elementos (mucho más intuitivo):
> ```css
> *, *::before, *::after {
>   box-sizing: border-box;
> }
> ```
> Incluye los pseudoelementos `::before`/`::after` para que también hereden el modelo. El detalle de por qué en [[06 box-sizing | box-sizing]].

## Especificidad cero

`*` no añade especificidad: cuenta como 0. Por eso un estilo puesto con `*` lo sobrescribe **cualquier** otro selector (un tipo, una clase). Es justo lo que se quiere para estilos base universales: que sean el punto de partida, no la última palabra.

## Combinarlo

El universal se combina para "todos los descendientes de…" o "todos los hijos de…":

```css
.tarjeta * { color: inherit; }   /* todos los descendientes de .tarjeta */
nav > *    { display: inline-block; }   /* todos los hijos directos de nav */
```

## Cuidado con el rendimiento (matizado)

> [!info] No es tan lento como se decía
> Antiguamente se advertía que `*` era lento. En los navegadores modernos, el selector universal **no es un problema de rendimiento** real en la práctica. La precaución válida no es de velocidad sino de **alcance**: `*` con propiedades heredables (como `color`) o costosas aplicadas a miles de elementos puede tener efectos amplios e inesperados. Úsalo con intención.

## Buenas prácticas

> [!tip] Recomendaciones
> - Úsalo para resets globales (`box-sizing`, `margin: 0`).
> - Inclúyelo con `*::before, *::after` cuando reinicies el box model.
> - Aprovecha su especificidad cero para estilos base sobreescribibles.
> - No lo cargues de muchas propiedades heredables sin pensar el alcance.

## Errores comunes

> [!warning] Trampas
> - **Propiedades heredables en `*`** que se propagan más de lo esperado.
> - **Olvidar `::before`/`::after`** al resetear `box-sizing`.

## Notas relacionadas

- [[01 Selector de Tipo]] — más acotado que el universal.
- [[06 box-sizing]] — el reset estrella con `*`.
- [[01 Cálculo de Especificidad]] — la especificidad cero del universal.
