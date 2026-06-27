---
title: margin — Espacio exterior de la caja
aliases:
  - margin
tags:
  - css
  - api/propiedad
  - layout
propiedad: margin
grupo: box-model
valor_inicial: "0"
hereda: false
animable: true
draft: false
order: 3
---

# Margin

> [!definicion]
> `margin` es el **espacio exterior** de una caja: separa el elemento de sus vecinos. A diferencia del [[03 Padding | padding]], está **fuera** del borde, es **siempre transparente**, no recibe eventos, y los márgenes verticales pueden **colapsar** entre sí.

```css
.parrafo { margin-block: 0 1rem; }      /* separación inferior entre párrafos */
.centrado { margin-inline: auto; }      /* centra horizontalmente */
```

## El shorthand (igual que padding)

Acepta de 1 a 4 valores en sentido horario, o propiedades por lado:

| Valores | Interpretación |
|---------|----------------|
| `margin: 1rem` | Los 4 lados iguales |
| `margin: 1rem 2rem` | Vertical · Horizontal |
| `margin: 1rem 2rem 3rem 4rem` | Arriba · Derecha · Abajo · Izquierda |

```css
margin-block: 1rem;     /* lógico: arriba + abajo */
margin-inline: auto;    /* lógico: izq + der */
margin-top: 0;          /* un lado */
```

## margin: auto centra horizontalmente

> [!tip] El truco clásico de centrado
> `margin-inline: auto` (o `margin: 0 auto`) **centra** un elemento de bloque horizontalmente, repartiendo el espacio sobrante a ambos lados por igual. Requiere que el elemento tenga un **ancho** definido (si no, ya ocupa todo y no hay nada que centrar):
> ```css
> .contenedor { width: 100%; max-width: 60rem; margin-inline: auto; }
> ```
> Es la forma estándar de centrar un contenedor. **Solo funciona horizontalmente** en flujo normal: `margin: auto` vertical **no** centra (salvo en Flexbox/Grid, donde sí).

## Margin negativo

`margin` admite valores **negativos**, que **acercan** o solapan elementos, tiran de un elemento hacia fuera de su contenedor o compensan el padding de un padre:

```css
.bleed { margin-inline: -1rem; }   /* el elemento "sangra" más allá del padre */
```

Es potente pero delicado: usado sin cuidado, crea solapamientos difíciles de depurar.

## El colapso de márgenes

> [!warning] Los márgenes verticales colapsan
> El comportamiento más sorprendente de `margin`: los márgenes **verticales** de elementos adyacentes (o de padre e hijo) **se fusionan** en uno solo —el mayor de los dos— en vez de sumarse. Dos párrafos con `margin: 20px` no quedan a 40px, sino a **20px**. Esto solo ocurre en el flujo normal con márgenes verticales (no en Flexbox/Grid, no en horizontal). Es importante y tiene su propia nota: [[07 Margin Collapse]].

## margin no recibe el fondo ni eventos

Como el margin es espacio exterior transparente:
- **No** se pinta del color de fondo (siempre deja ver lo de detrás).
- **No** recibe `:hover` ni clics.

Por eso, para agrandar el área clicable de un elemento, se usa **padding**, no margin.

## Buenas prácticas

> [!tip] Recomendaciones
> - `margin` para el espacio **entre** elementos; `padding` para el interior.
> - `margin-inline: auto` para centrar contenedores con ancho definido.
> - Prefiere márgenes en **un** sentido (p. ej. solo `margin-bottom` entre elementos, o el patrón [[03 Hermano Adyacente (+) | `* + *`]]) para evitar sorpresas del colapso.
> - Usa propiedades lógicas (`margin-block`/`margin-inline`) para soportar RTL.

## Errores comunes

> [!warning] Trampas
> - **Esperar que `margin: auto` centre verticalmente** en flujo normal: no lo hace (sí en flex/grid).
> - **Sumar márgenes verticales** sin contar el colapso: quedan en el mayor, no en la suma.
> - **Margin para el área clicable**: no recibe eventos; usa padding.

## Notas relacionadas

- [[03 Padding]] — el espacio interior, su contraparte.
- [[07 Margin Collapse]] — el colapso de márgenes verticales en detalle.
- [[05 Eje Transversal (align-items, align-self)]] — `margin: auto` en Flexbox sí centra en ambos ejes.
