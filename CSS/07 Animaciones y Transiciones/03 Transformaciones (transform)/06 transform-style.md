---
title: transform-style — Mantener el 3D en elementos anidados
aliases:
  - transform-style
  - preserve-3d
tags:
  - css
  - api/propiedad
  - animacion
propiedad: transform-style
grupo: animacion
valor_inicial: flat
hereda: false
animable: false
draft: false
order: 6
---

# transform-style

> [!definicion]
> `transform-style` decide si los **hijos** de un elemento transformado en 3D mantienen su posición tridimensional (`preserve-3d`) o se aplanan al plano del padre (`flat`). Es imprescindible para construir escenas 3D con varios elementos anidados, como una flip card o un cubo.

```css
.escena { perspective: 1000px; }
.tarjeta { transform-style: preserve-3d; }   /* las caras mantienen su 3D */
```

## flat vs. preserve-3d

| Valor | Comportamiento |
|-------|----------------|
| `flat` | Los hijos se **aplanan** al plano del elemento (por defecto) |
| `preserve-3d` | Los hijos **mantienen** su posición en el espacio 3D |

## Por qué hace falta preserve-3d

> [!warning] Sin preserve-3d, el 3D anidado se rompe
> Por defecto (`flat`), cuando transformas un elemento en 3D, sus hijos se **proyectan** sobre el plano del padre, perdiendo su propia profundidad. Para que los hijos (las caras de una tarjeta, las caras de un cubo) mantengan sus posiciones 3D **relativas al espacio**, el padre necesita `preserve-3d`:
> ```css
> /* La tarjeta gira; sus caras deben mantener el 3D */
> .tarjeta {
>   transform-style: preserve-3d;
>   transform: rotateY(180deg);
> }
> .cara-frontal, .cara-trasera { position: absolute; inset: 0; }
> .cara-trasera { transform: rotateY(180deg); }
> ```
> Sin `preserve-3d`, las caras se aplanarían y el efecto de volteo no funcionaría.

## El cubo 3D

> [!tip] Un cubo con preserve-3d
> El ejemplo didáctico clásico: un cubo cuyas seis caras se colocan en 3D con `translateZ` y `rotate`, dentro de un contenedor `preserve-3d`:
> ```css
> .cubo { transform-style: preserve-3d; }
> .cara { position: absolute; }
> .frente  { transform: translateZ(100px); }
> .atras   { transform: rotateY(180deg) translateZ(100px); }
> .derecha { transform: rotateY(90deg) translateZ(100px); }
> /* ...y las otras tres caras */
> ```
> Cada cara se mantiene en su posición 3D gracias a `preserve-3d` en el padre.

## El conflicto con overflow y otros

> [!warning] Algunas propiedades fuerzan flat
> `preserve-3d` es frágil: ciertas propiedades en el mismo elemento lo **anulan** silenciosamente y vuelven a `flat`, rompiendo el 3D. Las más comunes: `overflow` distinto de `visible`, `filter`, `clip-path`, `mask`, `opacity` < 1. Si una escena 3D se "aplana" sin razón, revisa si alguna de estas propiedades está en el elemento con `preserve-3d`.

## Buenas prácticas

> [!tip] Recomendaciones
> - Pon `transform-style: preserve-3d` en el contenedor de una escena 3D anidada (flip card, cubo).
> - Evita `overflow`/`filter`/`opacity` en el mismo elemento (anulan el preserve-3d).
> - Combínalo con `perspective` (en el abuelo) y `backface-visibility` en las caras.
> - Para 3D simple de un solo elemento, no hace falta `preserve-3d`.

## Errores comunes

> [!warning] Trampas
> - **Olvidar `preserve-3d`**: el 3D anidado se aplana.
> - **`overflow: hidden`** en el elemento `preserve-3d`: lo fuerza a `flat`.
> - **Esperar 3D anidado** sin un padre `preserve-3d`.

## Notas relacionadas

- [[03 Transformaciones 3D (translateZ, rotateX, scale3d)]] — las transformaciones 3D que requiere.
- [[04 perspective y perspective-origin]] — la perspectiva de la escena.
- [[07 backface-visibility]] — ocultar la cara trasera.
