---
title: CSS en Línea (atributo style) — Estilos en un solo elemento
aliases:
  - inline css
  - style attribute
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# CSS en Línea (atributo style)

> [!definicion]
> El CSS **en línea** se aplica con el atributo `style` directamente sobre **un** elemento. Funciona, pero se considera **mala práctica** general: no se reutiliza, mezcla presentación con estructura y tiene la especificidad más alta de todas (difícil de sobrescribir).

```html
<p style="color: crimson; font-size: 1.2rem;">Texto</p>
```

## Por qué evitarlo

| Problema | Detalle |
|----------|---------|
| No reutilizable | Hay que repetirlo en cada elemento |
| Mezcla capas | Mete CSS en el HTML |
| Especificidad altísima | Gana a casi toda regla; difícil de sobrescribir |
| Sin estados ni media queries | No admite `:hover`, `@media`, etc. |

El detalle de su especificidad pertenece a [[01 Cálculo de Especificidad | la nota de especificidad]]: el estilo en línea solo lo supera un `!important`.

## La alternativa: clases

```html
<!-- ❌ en línea -->
<p style="color: crimson; font-size: 1.2rem;">Texto</p>

<!-- ✅ clase + CSS -->
<p class="destacado">Texto</p>
```
```css
.destacado { color: crimson; font-size: 1.2rem; }
```

## Cuándo sí tiene sentido

> [!info] Los pocos usos legítimos
> - **Valores dinámicos calculados por JavaScript**: `style="width: 73%"`, una posición que cambia en tiempo real.
> - **Custom properties por elemento**: `style="--color: #cba6f7"` (variable que el CSS usa).
> - **Email HTML**: muchos clientes solo respetan estilos inline.
> - **Prototipos** de usar y tirar.
>
> Fuera de eso, clase + CSS.

## Buenas prácticas

> [!tip] Recomendaciones
> - Por defecto, estila con **clases**, no con `style`.
> - Reserva el inline para valores dinámicos de JS, variables CSS y email.
> - Para alternar apariencia, cambia clases con JS, no escribas estilos inline.

## Errores comunes

> [!warning] Trampas
> - **Estilar todo inline**: imposible de mantener.
> - **Pelear con su especificidad** desde la hoja (acabas necesitando `!important`).

## Notas relacionadas

- [[01 CSS Externo (link)]] · [[02 CSS Interno (style)]] — las alternativas con selectores.
- [[01 Cálculo de Especificidad]] — por qué el inline gana casi siempre.
- [[02 Estilo en Línea (style)]] — el atributo `style` desde HTML.
