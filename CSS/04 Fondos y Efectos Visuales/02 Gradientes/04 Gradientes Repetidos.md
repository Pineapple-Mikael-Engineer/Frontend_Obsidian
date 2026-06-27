---
title: Gradientes Repetidos — Patrones en bucle
aliases:
  - repeating gradients
  - repeating-linear-gradient
tags:
  - css
  - api/funcion
  - fondos
draft: false
order: 4
---

# Gradientes Repetidos

> [!definicion]
> Las funciones `repeating-linear-gradient()`, `repeating-radial-gradient()` y `repeating-conic-gradient()` **repiten** un patrón de gradiente en bucle a lo largo de toda la caja. Sirven para crear rayas, cuadrículas, patrones de líneas y texturas geométricas sin imágenes ni `background-size`.

```css
background: repeating-linear-gradient(45deg, #cba6f7 0 10px, #b48ef0 10px 20px);
```

## Cómo funciona la repetición

> [!info] El patrón se define una vez y se repite
> En un gradiente normal, los stops llegan hasta el final de la caja. En uno **repetido**, defines un **ciclo corto** (los stops llegan, por ejemplo, hasta 20px) y ese tramo se **repite** indefinidamente:
> ```css
> repeating-linear-gradient(
>   90deg,
>   #cba6f7 0 10px,    /* franja morada de 0 a 10px */
>   #b48ef0 10px 20px  /* franja lila de 10 a 20px */
> )
> /* el tramo de 20px se repite por toda la caja */
> ```
> A diferencia de los patrones con `background-size`, aquí la repetición está **dentro** del gradiente.

## Recetas de patrones

```css
/* Rayas diagonales */
.rayas {
  background: repeating-linear-gradient(45deg,
    #cba6f7 0 12px, #b48ef0 12px 24px);
}

/* Líneas finas horizontales (papel rayado) */
.rayado {
  background: repeating-linear-gradient(
    #fff 0 28px, #e0e0f0 28px 30px);
}

/* Patrón de cebra para zonas de "construcción" */
.precaucion {
  background: repeating-linear-gradient(45deg,
    #000 0 20px, #ffd000 20px 40px);
}

/* Anillos concéntricos */
.diana {
  background: repeating-radial-gradient(circle,
    #cba6f7 0 10px, #1e1e2e 10px 20px);
}

/* Rayos desde el centro (cónico repetido) */
.rayos {
  background: repeating-conic-gradient(#cba6f7 0 10deg, #1e1e2e 10deg 20deg);
}
```

## repeating vs. background-size

> [!info] Dos formas de hacer patrones
> Hay dos maneras de crear patrones repetidos:
> - **Gradiente repetido** (`repeating-*`): la repetición está en el gradiente. Bueno para rayas y líneas.
> - **Gradiente normal + `background-size`**: defines una celda pequeña que `background-repeat` repite. Bueno para puntos y cuadrículas.
>
> Para rayas, `repeating-linear-gradient` es más directo; para puntos, el patrón con `background-size` (ver [[02 radial-gradient | radial-gradient]]) suele ser más simple.

## Cuidado con el aliasing

> [!warning] Las franjas muy finas pueden verse borrosas
> Con franjas de pocos píxeles, los bordes entre colores pueden **emborronarse** (anti-aliasing) en lugar de quedar nítidos, sobre todo en ángulos. Para rayas nítidas, usa transiciones duras (stops en la misma posición) y, si hace falta, ajusta los tamaños a valores que cuadren con la rejilla de píxeles.

## Buenas prácticas

> [!tip] Recomendaciones
> - `repeating-linear-gradient` para rayas y líneas; el patrón con `background-size` para puntos.
> - Usa transiciones duras (stops en la misma posición) para franjas nítidas.
> - Define el patrón en variables para reutilizarlo y tematizarlo.
> - Verifica la nitidez de las franjas finas (posible aliasing en ángulos).

## Errores comunes

> [!warning] Trampas
> - **Franjas borrosas** por anti-aliasing en ángulos.
> - **Olvidar que el ciclo se repite**: definir stops hasta el 100% no repite nada (sería un gradiente normal).
> - **Patrones que no casan** entre repeticiones si los tamaños no cuadran.

## Notas relacionadas

- [[01 linear-gradient]] — la versión no repetida.
- [[09 Múltiples Fondos]] — combinar patrones en capas.
- [[03 background-repeat]] — la otra forma de repetir (con `background-size`).
