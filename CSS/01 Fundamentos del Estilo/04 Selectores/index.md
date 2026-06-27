---
title: Selectores — Apuntar a los elementos a estilar
aliases:
  - css selectors
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
order: 3
---

# Selectores

> [!definicion]
> Un **selector** decide a **qué** elementos del HTML se aplica una regla. Es la mitad "a qué" de cada regla CSS. Los hay básicos (por tipo, clase, id), combinadores (por relación entre elementos) y de atributo, además de las [[08 Pseudoclases y Pseudoelementos/index | pseudoclases y pseudoelementos]] que tienen su propia sección.

```css
h1            { }   /* por tipo */
.boton        { }   /* por clase */
#menu         { }   /* por id */
nav > a       { }   /* combinador: hijo directo */
a[target]     { }   /* por atributo */
```

## Mapa de la subsección

- [[01 Selectores Básicos/index]] — tipo, clase, id, universal, agrupación.
- [[02 Combinadores/index]] — descendente, hijo directo `>`, hermano `+` y `~`.
- [[03 Selectores de Atributo]] — `[attr]`, `[attr=valor]`, `^=`, `$=`, `*=`…

## El selector y la especificidad

> [!info] El tipo de selector decide quién gana
> Cuando varias reglas afectan al mismo elemento, el navegador resuelve el conflicto por **especificidad**, y esta depende del **tipo de selector** usado: un id pesa más que una clase, y una clase más que un tipo. Por eso elegir el selector no es solo "a qué apunto", sino también "cuánta prioridad tiene". El cálculo completo está en [[01 Cálculo de Especificidad | Especificidad]].
> | Selector | Peso de especificidad |
> |----------|----------------------|
> | Tipo (`h1`), pseudoelemento | Bajo |
> | Clase (`.x`), atributo (`[x]`), pseudoclase (`:hover`) | Medio |
> | Id (`#x`) | Alto |

## Combinar selectores

Los selectores se combinan para apuntar con precisión:

```css
.menu li.activo a:hover { }
/* enlace, en hover, dentro de un li con clase activo, dentro de .menu */
```

Cuanto más específica la combinación, más prioridad y menos reutilizable. El equilibrio entre precisión y mantenibilidad es un tema de [[09 Arquitectura y Metodologías/index | arquitectura]].

## Buenas prácticas

> [!tip] Recomendaciones
> - Prefiere selectores de **clase**: precisos pero de especificidad manejable.
> - Evita encadenar muchos niveles (`.a .b .c .d`): frágil y específico.
> - Reserva los `#id` para casos puntuales (su especificidad alta complica).
> - Apunta por **función** (`.boton-primario`), no por contexto frágil (`div > div > span`).

## Notas relacionadas

- [[01 Selectores Básicos/index]] — el punto de partida.
- [[02 Combinadores/index]] — relaciones entre elementos.
- [[01 Cálculo de Especificidad]] — cómo los selectores compiten.
