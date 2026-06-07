---
title: nth-child() — El enésimo hijo (con fórmulas)
aliases:
  - ":nth-child"
  - nth-child
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":nth-child"
grupo: pseudoclase
draft: false
---

# :nth-child()

> [!definicion]
> `:nth-child(n)` selecciona elementos según su **posición** entre sus hermanos, usando un número, las palabras `odd`/`even`, o una **fórmula** `an+b`. Es la pseudoclase estructural más potente: permite seleccionar patrones repetidos (filas alternas, cada tercer elemento, rangos).

```css
tr:nth-child(even) { background: #f5f5f5; }   /* filas pares */
li:nth-child(3n)   { color: #cba6f7; }         /* cada tercero */
```

## Las formas del argumento

| Argumento | Selecciona |
|-----------|------------|
| `3` | El tercer hijo exactamente |
| `odd` | Los impares (1, 3, 5…) |
| `even` | Los pares (2, 4, 6…) |
| `2n` | Cada segundo (= even) |
| `3n` | Cada tercero (3, 6, 9…) |
| `3n+1` | El 1, 4, 7… (cada tercero empezando por el 1) |
| `n+4` | Del cuarto en adelante |
| `-n+3` | Los **tres primeros** |

## La fórmula an+b

> [!info] Cómo leer 3n+1
> En `an+b`, `n` toma los valores 0, 1, 2, 3… y la fórmula calcula las posiciones:
> - `3n+1` → con n=0,1,2…: posiciones **1, 4, 7, 10**…
> - `2n` → posiciones **2, 4, 6**… (los pares)
> - `n+4` → posiciones **4, 5, 6**… (del cuarto en adelante)
> - `-n+3` → posiciones **3, 2, 1** (los tres primeros)
>
> Combinando el signo y los valores se seleccionan rangos, patrones e intervalos. Las herramientas online ("nth-child tester") ayudan a visualizarlos.

## Recetas comunes

```css
/* Filas de tabla alternas (cebra) */
tr:nth-child(odd) { background: #fafafa; }

/* Los tres primeros elementos destacados */
.lista li:nth-child(-n+3) { font-weight: bold; }

/* Todos menos el primero */
li:nth-child(n+2) { border-top: 1px solid #eee; }

/* Cada cuarto elemento (rejilla con acento) */
.grid > :nth-child(4n) { background: #cba6f7; }

/* El penúltimo */
li:nth-last-child(2) { }
```

## nth-last-child: contar desde el final

> [!tip] La versión que cuenta al revés
> `:nth-last-child()` funciona igual pero **desde el final**: `:nth-last-child(1)` es el último, `:nth-last-child(2)` el penúltimo. Útil para los últimos N elementos:
> ```css
> li:nth-last-child(-n+3) { }   /* los tres últimos */
> ```

## El truco de "cantidad" (quantity queries)

> [!info] Estilar según cuántos elementos hay
> Combinando `:nth-child` con `:nth-last-child` se pueden hacer "quantity queries": estilar elementos **según cuántos hermanos hay** en total. Por ejemplo, aplicar un layout distinto solo si hay **exactamente 3** elementos:
> ```css
> li:first-child:nth-last-child(3),
> li:first-child:nth-last-child(3) ~ li { /* solo cuando hay 3 en total */ }
> ```
> Es avanzado pero muestra el poder de combinar estas pseudoclases.

## child vs. of-type

> [!warning] nth-child cuenta TODOS los hermanos
> Como [[01 first-child y last-child | `:first-child`]], `:nth-child` cuenta **todos** los hijos sin importar el tipo. Si hay elementos de distintos tipos mezclados, usa [[04 nth-of-type() | `:nth-of-type()`]] para contar solo los del mismo tipo.

## Buenas prácticas

> [!tip] Recomendaciones
> - `:nth-child(odd/even)` para filas de tabla alternas (legibilidad).
> - Usa las fórmulas `an+b` para patrones; verifica con un "nth-child tester".
> - `:nth-last-child` para contar desde el final.
> - Recuerda que cuenta todos los hermanos; usa `:nth-of-type` si el tipo importa.

## Errores comunes

> [!warning] Trampas
> - **Fórmula equivocada**: `3n` vs. `3n+1` seleccionan posiciones distintas.
> - **Contar el tipo equivocado**: usa `:nth-of-type` si hay tipos mezclados.
> - **Off-by-one**: `n` empieza en 0, así que `3n` da 0, 3, 6… (la posición 0 no existe, empieza en 3).

## Notas relacionadas

- [[04 nth-of-type()]] — contar solo los del mismo tipo.
- [[01 first-child y last-child]] — los casos `:nth-child(1)`/`:nth-last-child(1)`.
- [[04 not()]] — combinar para excluir.
