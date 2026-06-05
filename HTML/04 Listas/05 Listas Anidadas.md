---
title: Listas Anidadas — Listas dentro de listas
aliases:
  - nested lists
  - listas anidadas
tags:
  - html
  - api/concepto
  - semantica
draft: false
---

# Listas Anidadas

> [!definicion]
> Una lista anidada es una lista (`<ul>` u `<ol>`) colocada **dentro de un [[03 Elementos de Lista (li) | `<li>`]]** de otra lista, para representar una jerarquía: subapartados, un árbol de categorías, un índice con niveles. La regla de oro: la sublista va **dentro del `<li>` padre**, no suelta entre ítems.

```html
<ul>
  <li>Frutas
    <ul>
      <li>Cítricos
        <ul>
          <li>Naranja</li>
          <li>Limón</li>
        </ul>
      </li>
      <li>Tropicales</li>
    </ul>
  </li>
  <li>Verduras</li>
</ul>
```

## La regla estructural

> [!warning] La sublista va dentro del li, no entre li
> El error más común al anidar:
> ```html
> <!-- ❌ La sublista cuelga del ul, no del li -->
> <ul>
>   <li>Frutas</li>
>   <ul><li>Naranja</li></ul>   <!-- inválido: ul dentro de ul -->
> </ul>
>
> <!-- ✅ La sublista está dentro del li al que pertenece -->
> <ul>
>   <li>Frutas
>     <ul><li>Naranja</li></ul>
>   </li>
> </ul>
> ```
> Un `<ul>`/`<ol>` solo acepta `<li>` como hijo directo; por eso la sublista debe estar **dentro** de un `<li>`, nunca como hermana de los `<li>`.

## Mezclar tipos

Se pueden combinar listas ordenadas y no ordenadas según el significado de cada nivel:

```html
<ol>
  <li>Preparar la masa
    <ul>
      <li>250 g de harina</li>
      <li>2 huevos</li>
    </ul>
  </li>
  <li>Hornear 30 minutos</li>
</ol>
```

Aquí los pasos van numerados (`<ol>`) y los ingredientes de cada paso, sin orden (`<ul>`).

## Estilo por nivel

El navegador cambia automáticamente la viñeta según el nivel de anidación (•, ◦, ▪). Con CSS se controla por profundidad:

```css
ul { list-style-type: disc; }
ul ul { list-style-type: circle; }
ul ul ul { list-style-type: square; }
```

Para listas ordenadas anidadas con numeración compuesta (1, 1.1, 1.2), se usan **contadores CSS**.

## Accesibilidad

> [!info] La jerarquía se anuncia
> Los lectores de pantalla anuncian el **nivel de anidación** ("lista de 2 elementos, nivel 2"), lo que permite a quien no ve la pantalla entender la jerarquía. Una estructura bien anidada (sublistas dentro de su `<li>`) transmite esa jerarquía; una mal formada la rompe.

## Buenas prácticas

> [!tip] Recomendaciones
> - Coloca cada sublista **dentro** del `<li>` al que pertenece.
> - Mezcla `<ul>` y `<ol>` según el significado de cada nivel.
> - No abuses de la profundidad: más de 3-4 niveles suele indicar que conviene otra estructura.

## Errores comunes

> [!warning] Trampas
> - **Sublista hermana de los `<li>`** en vez de dentro de uno: marcado inválido.
> - **Anidar demasiado**: jerarquías muy profundas son difíciles de leer y de navegar.

## Notas relacionadas

- [[03 Elementos de Lista (li)]] — el ítem que contiene la sublista.
- [[01 Listas Ordenadas (ol)]] · [[02 Listas No Ordenadas (ul)]] — los tipos que se combinan al anidar.
