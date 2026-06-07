---
title: Transformaciones 2D — translate, rotate, scale, skew
aliases:
  - translate
  - rotate
  - scale
  - skew
tags:
  - css
  - api/funcion
  - animacion
draft: false
---

# Transformaciones 2D (translate, rotate, scale, skew)

> [!definicion]
> Las cuatro transformaciones 2D básicas son: **`translate`** (mover), **`rotate`** (girar), **`scale`** (escalar) y **`skew`** (inclinar/deformar). Se aplican con la función `transform` y se pueden combinar encadenándolas.

```css
.x {
  transform: translate(20px, 10px) rotate(15deg) scale(1.2);
}
```

## translate: mover

| Función | Efecto |
|---------|--------|
| `translateX(20px)` | Mueve en horizontal |
| `translateY(-10px)` | Mueve en vertical |
| `translate(20px, -10px)` | Ambos ejes |
| `translate(50%, 50%)` | El % es del **propio elemento** (clave para centrar) |

> [!tip] El truco de centrado translate(-50%, -50%)
> `translate` con `%` se refiere al **tamaño del propio elemento**, lo que da la receta clásica de centrado de tamaño desconocido:
> ```css
> .centro {
>   position: absolute;
>   top: 50%; left: 50%;
>   transform: translate(-50%, -50%);   /* se mueve hacia atrás la mitad de su tamaño */
> }
> ```

## rotate: girar

`rotate(<ángulo>)` gira el elemento alrededor de su [[05 transform-origin | centro]] (por defecto):

```css
transform: rotate(45deg);    /* 45° en sentido horario */
transform: rotate(-90deg);   /* 90° antihorario */
transform: rotate(0.25turn); /* en vueltas */
```

## scale: escalar

| Función | Efecto |
|---------|--------|
| `scale(1.5)` | Agranda al 150% en ambos ejes |
| `scale(2, 0.5)` | 2x horizontal, 0.5x vertical |
| `scaleX(0)` | Colapsa horizontalmente (a 0) |

> [!info] scale no afecta al layout (a diferencia de width)
> `scale(1.5)` agranda **visualmente** el elemento sin cambiar su tamaño en el flujo (no empuja a los vecinos), y es **eficiente** de animar. Por eso, para un efecto de "crecer al pasar el ratón", `transform: scale()` es muy superior a animar `width`/`height` (que provocan reflow):
> ```css
> .card:hover { transform: scale(1.05); }   /* fluido, no descuadra el layout */
> ```

## skew: inclinar

`skew` deforma el elemento inclinándolo, como un paralelogramo:

```css
transform: skewX(15deg);    /* inclina horizontalmente */
transform: skew(10deg, 5deg);
```

Es el menos usado; sirve para efectos de diseño (banners inclinados, fondos diagonales).

## Recetas comunes

```css
/* Botón que se eleva y crece sutilmente */
.btn { transition: transform 0.2s; }
.btn:hover { transform: translateY(-2px) scale(1.02); }

/* Icono que gira al expandir un acordeón */
.flecha { transition: transform 0.2s; }
.abierto .flecha { transform: rotate(180deg); }

/* Centrado absoluto de tamaño desconocido */
.modal { position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); }

/* Spinner que gira en bucle */
.spinner { animation: girar 1s linear infinite; }
@keyframes girar { to { transform: rotate(360deg); } }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `transform` para mover/escalar en animaciones (eficiente, no provoca reflow).
> - `translate(-50%, -50%)` para centrar elementos de tamaño desconocido.
> - `scale()` en hover en vez de animar `width`/`height`.
> - Recuerda que el orden de las funciones encadenadas importa.

## Errores comunes

> [!warning] Trampas
> - **Animar `width`/`top`** en vez de `scale`/`translate`: tirones.
> - **Orden de funciones** que da un resultado inesperado.
> - **Esperar que `scale` empuje** a los vecinos: no afecta al layout.

## Notas relacionadas

- [[03 Transformaciones 3D (translateZ, rotateX, scale3d)]] — el espacio 3D.
- [[05 transform-origin]] — cambiar el punto de pivote.
- [[03 Animar transform y opacity]] — por qué es eficiente.
