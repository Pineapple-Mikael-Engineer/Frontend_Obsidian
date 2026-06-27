---
title: where() — Agrupar sin especificidad
aliases:
  - ":where"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":where"
grupo: pseudoclase
draft: false
order: 2
---

# :where()

> [!definicion]
> `:where(selector, …)` agrupa selectores igual que [[01 is() | `:is()`]], pero con una diferencia crucial: su **especificidad es siempre cero**. Es perfecto para estilos base que deben ser **fáciles de sobrescribir** sin guerras de especificidad.

```css
:where(h1, h2, h3) { margin: 0; }   /* fácil de sobrescribir luego */
```

## La diferencia con :is(): especificidad cero

> [!info] :where() no aporta peso
> `:is()` y `:where()` agrupan igual, pero:
> - **`:is(.a, #b)`** → especificidad de `#b` (alta).
> - **`:where(.a, #b)`** → especificidad **0,0,0** (nula).
>
> Todo lo que va dentro de `:where()` cuenta como **cero** para la cascada. La regla aún se aplica, pero **cualquier** otro selector la sobrescribe con facilidad.

## El uso estrella: estilos base y librerías

> [!tip] :where() para resets y CSS de librería
> `:where()` brilla en **resets** y **librerías de componentes**, donde quieres dar estilos por defecto que el usuario pueda sobrescribir **sin esfuerzo**:
> ```css
> /* Reset con especificidad cero: el usuario sobrescribe sin pelear */
> :where(ul, ol) { margin: 0; padding: 0; list-style: none; }
> ```
> ```css
> /* En una librería: estilos base que cualquier clase del usuario gana */
> :where(.btn) { padding: 0.5rem 1rem; border-radius: 4px; }
> .mi-boton { padding: 1rem; }   /* gana fácilmente, aunque venga "después" o no */
> ```
> Sin `:where()`, sobrescribir el CSS de una librería suele requerir aumentar la especificidad o usar `!important`. Con `:where()`, el usuario manda siempre.

## El patrón de reset moderno

> [!tip] Resets que no estorban
> Los resets modernos usan `:where()` para que sus reglas **nunca** compitan con el CSS del autor:
> ```css
> :where(img, picture, video) { max-width: 100%; height: auto; }
> :where(h1, h2, h3, h4) { text-wrap: balance; }
> ```
> Cualquier estilo posterior del desarrollador gana sin pensar en la especificidad. Es la diferencia entre un reset que ayuda y uno que estorba.

## :where() para anidar sin penalización

Como su especificidad es cero, `:where()` permite agrupar contextos sin que la anidación dispare la especificidad:

```css
:where(.tema-oscuro) :where(.card) { background: #1e1e2e; }   /* especificidad 0 */
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `:where()` para estilos base/resets que deban sobrescribirse con facilidad.
> - Ideal en librerías de componentes: el usuario gana sin pelear con la especificidad.
> - Usa `:is()` (no `:where()`) cuando **sí** quieras mantener el peso del selector.
> - Combínalo con [[06 Capas (@layer)]] para un control total de la cascada.

## Errores comunes

> [!warning] Trampas
> - **Usar `:where()`** donde necesitabas que el estilo pesara (usa `:is()`).
> - **Esperar que `:where()` "gane"**: tiene especificidad cero, casi todo lo sobrescribe.

## Notas relacionadas

- [[01 is()]] — la versión que mantiene la especificidad.
- [[06 Capas (@layer)]] — otra herramienta para controlar la cascada.
- [[01 Especificidad/index]] — por qué la especificidad cero importa.
