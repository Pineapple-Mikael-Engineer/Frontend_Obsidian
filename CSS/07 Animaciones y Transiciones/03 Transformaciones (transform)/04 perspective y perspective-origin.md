---
title: perspective y perspective-origin — La profundidad 3D
aliases:
  - perspective
  - perspective-origin
tags:
  - css
  - api/propiedad
  - animacion
propiedad: perspective
grupo: animacion
valor_inicial: none
hereda: false
animable: true
draft: false
---

# perspective y perspective-origin

> [!definicion]
> `perspective` define la **distancia** entre el espectador y el plano Z=0, lo que da a las transformaciones 3D su sensación de **profundidad**. Sin ella, el 3D se ve plano. `perspective-origin` define **desde dónde** se mira (el punto de fuga). Es el ingrediente que hace que `rotateY` parezca una puerta girando y no un aplastamiento.

```css
.escena { perspective: 800px; }
.escena .tarjeta { transform: rotateY(30deg); }   /* ahora con profundidad */
```

## El valor de perspective: la distancia

> [!info] Cuanto menor, más dramático
> `perspective` es la distancia del "ojo" a la pantalla. Un valor **pequeño** (200px) crea un efecto 3D **exagerado** (como mirar de muy cerca); uno **grande** (2000px) un efecto **sutil** (como mirar de lejos):
> | Valor | Efecto |
> |-------|--------|
> | `200px` | 3D muy dramático, casi de ojo de pez |
> | `800px–1200px` | Equilibrado (lo más usado) |
> | `2000px+` | Sutil, casi plano |
>
> Valores entre 800 y 1200px suelen dar el efecto más natural.

## Las dos formas de aplicar perspective

> [!warning] Propiedad perspective vs. función perspective()
> Hay dos maneras de dar perspectiva, con un efecto distinto:
> - **Propiedad `perspective` en el contenedor**: todos los hijos comparten **el mismo punto de fuga**, como una escena real con varios objetos. Coherente para múltiples elementos 3D.
> - **Función `perspective()` en el `transform` del elemento**: cada elemento tiene **su propia** perspectiva independiente. Para un solo elemento.
> ```css
> /* Escena compartida (varios elementos coherentes) */
> .escena { perspective: 1000px; }
> /* Perspectiva individual (un elemento) */
> .x { transform: perspective(1000px) rotateY(30deg); }
> ```
> Para una flip card o varios elementos 3D relacionados, la **propiedad** en el contenedor es lo correcto.

## perspective-origin: el punto de fuga

`perspective-origin` mueve el "ojo" del espectador, cambiando desde qué ángulo se ve la escena 3D:

```css
.escena {
  perspective: 1000px;
  perspective-origin: top right;   /* mirar desde arriba a la derecha */
}
```

Por defecto es `center` (mirar de frente). Moverlo crea efectos de ángulo.

## Buenas prácticas

> [!tip] Recomendaciones
> - Pon `perspective` en el **contenedor** para escenas con varios elementos 3D.
> - Usa la función `perspective()` para un solo elemento independiente.
> - Valores de 800–1200px para un efecto natural; menos para más dramatismo.
> - Ajusta `perspective-origin` para cambiar el ángulo de visión.

## Errores comunes

> [!warning] Trampas
> - **Transformaciones 3D sin `perspective`**: se ven planas.
> - **Perspectiva demasiado pequeña**: efecto de ojo de pez exagerado.
> - **Función vs. propiedad**: para varios elementos coherentes, usa la propiedad en el contenedor.

## Notas relacionadas

- [[03 Transformaciones 3D (translateZ, rotateX, scale3d)]] — las transformaciones que perspective hace visibles.
- [[06 transform-style]] — mantener el 3D en estructuras anidadas.
- [[07 backface-visibility]] — la cara trasera.
