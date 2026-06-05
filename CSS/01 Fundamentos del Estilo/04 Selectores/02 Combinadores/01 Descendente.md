---
title: Combinador Descendente — Elementos dentro de otro
aliases:
  - descendant combinator
tags:
  - css
  - api/selector
  - arquitectura
selector: descendente
draft: false
---

# Combinador Descendente

> [!definicion]
> El combinador **descendente** (un **espacio** entre dos selectores) selecciona elementos que están **dentro** de otro, a **cualquier** nivel de anidación. `A B` significa "todo `B` que descienda de un `A`".

```css
nav a { color: #cba6f7; }
/* todo <a> dentro de un <nav>, sin importar cuán anidado */
```

```html
<nav>
  <a>Nivel 1</a>            <!-- ✅ -->
  <ul><li><a>Nivel 3</a></li></ul>   <!-- ✅ también -->
</nav>
```

## A cualquier profundidad

La clave del descendente: no importa cuántos niveles haya entre el ancestro y el descendiente. Esto lo hace cómodo pero también de **alcance amplio**: afecta a todos los descendientes coincidentes, incluidos los anidados que quizá no querías.

## Encadenar varios

Se pueden encadenar para exigir una cadena de ancestros:

```css
.articulo .contenido p { line-height: 1.7; }
/* <p> dentro de .contenido dentro de .articulo */
```

> [!warning] No abuses de las cadenas largas
> Cadenas como `.a .b .c .d p` son **frágiles** (dependen de una estructura HTML concreta que puede cambiar) y de **especificidad creciente** (difíciles de sobrescribir). Si te ves encadenando muchos niveles, suele ser señal de que conviene una **clase** directa en el elemento objetivo.

## Descendente vs. hijo directo

| | `A B` (descendente) | `A > B` (hijo directo) |
|--|---------------------|------------------------|
| Profundidad | Cualquiera | Solo un nivel |
| Alcance | Amplio | Acotado |
| Predecibilidad | Menor | Mayor |

Cuando solo quieres los hijos inmediatos, usa [[02 Hijo Directo (>)|`>`]]; el descendente incluye a los nietos.

## Buenas prácticas

> [!tip] Recomendaciones
> - Úsalo para estilos de contexto ("enlaces dentro de la navegación").
> - Limita la profundidad de las cadenas; una o dos relaciones suele bastar.
> - Si la estructura es importante y acotada, prefiere `>`.
> - Ante cadenas muy largas, replantéate con una clase directa.

## Errores comunes

> [!warning] Trampas
> - **Alcance excesivo**: afecta a descendientes anidados no deseados.
> - **Cadenas frágiles** atadas a una estructura HTML concreta.
> - **Confundir el espacio con la coma** (`A B` ≠ `A, B`).

## Notas relacionadas

- [[02 Hijo Directo (>)]] — acotar a un solo nivel.
- [[index]] — el conjunto de combinadores.
- [[02 Selector de Clase]] — la alternativa a las cadenas largas.
