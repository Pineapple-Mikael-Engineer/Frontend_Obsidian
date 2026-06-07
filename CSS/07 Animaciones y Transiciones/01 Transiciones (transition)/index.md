---
title: Transiciones (transition) — Interpolar entre dos estados
aliases:
  - transition
tags:
  - css
  - api/concepto
  - animacion
draft: false
---

# Transiciones (transition)

> [!definicion]
> Una **transición** suaviza el cambio de una propiedad de un valor a otro a lo largo del tiempo, en vez de cambiar de golpe. Se activa cuando la propiedad cambia (por `:hover`, `:focus`, o una clase añadida con JS). El shorthand `transition` combina qué propiedad, cuánto dura, con qué curva y con qué retardo.

```css
.boton {
  transition: background 0.2s ease, transform 0.2s ease;
}
.boton:hover { background: #b48ef0; transform: scale(1.05); }
```

## El shorthand transition

```css
transition: <propiedad> <duración> <curva> <retardo>;
```

| Parte | Ejemplo |
|-------|---------|
| Propiedad | `background` |
| Duración | `0.2s` |
| Curva | `ease` |
| Retardo | `0.1s` |

Cada parte en su nota: [[01 transition-property]], [[02 transition-duration]], [[03 transition-timing-function]] y [[04 transition-delay]].

Solo la **duración** es obligatoria; el resto tiene valores por defecto.

## Mapa de la subsección

- [[01 transition-property]] — qué propiedades se animan.
- [[02 transition-duration]] — cuánto dura.
- [[03 transition-timing-function]] — la curva de aceleración.
- [[04 transition-delay]] — el retardo antes de empezar.
- [[05 Propiedades Animables]] — qué se puede (y no se puede) transicionar.

## La transición va en el estado base

> [!warning] Declara la transición en el estado normal, no en :hover
> Un error frecuente: poner la `transition` solo en `:hover`. Así la entrada es suave pero la salida es brusca (al quitar el ratón, no hay transición declarada). La `transition` debe ir en el estado **base** para que se aplique en **ambas** direcciones:
> ```css
> /* ✅ transición en el base: suave al entrar Y al salir */
> .btn { transition: transform 0.2s; }
> .btn:hover { transform: scale(1.1); }
>
> /* ❌ solo en hover: entra suave, sale de golpe */
> .btn:hover { transition: transform 0.2s; transform: scale(1.1); }
> ```

## Transicionar varias propiedades

Separadas por comas, cada una con su duración/curva:

```css
transition: background 0.2s ease, transform 0.3s ease-out, box-shadow 0.2s;
```

O todas con `all` (cómodo pero con cuidado por rendimiento):

```css
transition: all 0.2s;   /* anima cualquier propiedad que cambie */
```

> [!warning] Cuidado con transition: all
> `transition: all` anima **toda** propiedad que cambie, lo que puede incluir cosas costosas o inesperadas (un `height: auto` que se calcula, un reflow). Es cómodo para prototipos, pero en producción es mejor **listar** las propiedades concretas que quieres animar, por rendimiento y previsibilidad.

## Recetas comunes

```css
/* Botón con feedback de hover */
.btn { transition: background 0.2s, transform 0.1s; }
.btn:hover { background: #b48ef0; }
.btn:active { transform: scale(0.97); }   /* presionado */

/* Enlace con subrayado animado */
.link { transition: color 0.2s; }

/* Tarjeta que se eleva al pasar el ratón */
.card { transition: transform 0.2s, box-shadow 0.2s; }
.card:hover { transform: translateY(-4px); }
```

## Notas relacionadas

- [[01 transition-property]] — qué animar.
- [[05 Propiedades Animables]] — qué propiedades pueden transicionarse.
- [[04 Animaciones (@keyframes)/index]] — para secuencias más complejas.
