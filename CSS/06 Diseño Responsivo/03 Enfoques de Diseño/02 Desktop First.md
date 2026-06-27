---
title: Desktop First — Diseñar desde la pantalla grande
aliases:
  - desktop first
tags:
  - css
  - api/concepto
  - responsive
draft: false
order: 2
---

# Desktop First

> [!definicion]
> **Desktop-first** es la estrategia inversa al mobile-first: se diseña primero para la **pantalla grande** (escritorio) como base, y se **adapta** a pantallas menores con media queries de `max-width`. Es válido, pero hoy se considera menos óptimo que [[01 Mobile First | mobile-first]].

```css
/* Base: escritorio (tres columnas) */
.galeria { display: grid; grid-template-columns: repeat(3, 1fr); }

/* Se reduce en móvil */
@media (max-width: 767px) {
  .galeria { grid-template-columns: 1fr; }
}
```

## La estructura desktop-first

> [!info] Base compleja + max-width para reducir
> El patrón opuesto al mobile-first:
> 1. El **CSS base** describe la versión de escritorio: varias columnas, menú completo.
> 2. Las media queries de **`max-width`** **simplifican** al reducirse la pantalla: una columna, menú hamburguesa.
>
> Se construye "de más a menos": empiezas con todo y vas quitando o reorganizando.

## Por qué se prefiere mobile-first

> [!warning] Desktop-first suele dar más sobrescrituras
> Desktop-first tiene inconvenientes frente a mobile-first:
> - **Más sobrescrituras**: deshacer estilos de escritorio (`max-width`) suele requerir más reglas que añadirlos progresivamente.
> - **Peor rendimiento en móvil**: el dispositivo más limitado recibe el CSS más **complejo** por defecto y luego lo simplifica.
> - **Riesgo de olvidar casos**: es fácil que un componente de escritorio no se adapte bien a móvil si se piensa "al revés".
>
> Por eso la recomendación general es **mobile-first**, salvo casos concretos.

## Cuándo desktop-first tiene sentido

> [!info] Algunos contextos lo justifican
> Desktop-first puede ser razonable cuando:
> - El producto es **principalmente de escritorio** (una herramienta de administración, un editor complejo) y el móvil es secundario.
> - Se **rediseña** un sitio antiguo que ya era desktop-first y conviene mantener la coherencia.
> - El diseño de partida (de un diseñador) es solo de escritorio y hay que adaptarlo.

## El orden descendente

> [!warning] max-width de mayor a menor
> En desktop-first, las media queries de `max-width` se ordenan de **mayor a menor** para que la cascada funcione:
> ```css
> @media (max-width: 1024px) { … }   /* primero */
> @media (max-width: 767px)  { … }   /* después: gana en pantallas pequeñas */
> ```
> Es el orden inverso al de mobile-first.

## Buenas prácticas

> [!tip] Recomendaciones
> - Considera desktop-first solo si el producto es **principalmente de escritorio**.
> - Si lo usas, ordena los `max-width` de mayor a menor.
> - En general, prefiere **mobile-first**: CSS más limpio y mejor rendimiento móvil.
> - No mezcles los dos enfoques en el mismo proyecto sin una razón clara.

## Errores comunes

> [!warning] Trampas
> - **Más reglas de las necesarias** por deshacer estilos en vez de añadirlos.
> - **Olvidar adaptar** componentes complejos a móvil.
> - **Orden ascendente** de los `max-width`: rompe la cascada.

## Notas relacionadas

- [[01 Mobile First]] — el enfoque recomendado.
- [[03 Diseño Fluido]] — la alternativa que reduce ambos.
- [[01 Sintaxis @media]] — la sintaxis de `max-width`.
