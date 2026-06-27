---
title: "@container y container-type — Declarar y consultar un contenedor"
aliases:
  - container-type
  - "@container"
tags:
  - css
  - api/concepto
  - responsive
draft: false
order: 1
---

# @container y container-type

> [!definicion]
> Para usar [[index | container queries]] hacen falta dos pasos: marcar un elemento como **contenedor de consulta** con `container-type` (o el atajo `container`), y luego escribir reglas `@container` que se aplican según el tamaño de ese contenedor. Es el equivalente de `@media`, pero relativo al contenedor en vez del viewport.

```css
.tarjeta-wrap { container-type: inline-size; }

@container (min-width: 25rem) {
  .tarjeta { display: grid; grid-template-columns: auto 1fr; }
}
```

## Paso 1: declarar el contenedor

> [!info] container-type marca quién se puede consultar
> Primero se marca el elemento cuyo tamaño se quiere consultar. El valor más usado es `inline-size` (consultar solo el ancho):
> ```css
> .contenedor { container-type: inline-size; }
> ```
> | Valor | Consulta |
> |-------|----------|
> | `inline-size` | El **ancho** (lo más común) |
> | `size` | Ancho **y** alto (requiere que el contenedor tenga altura definida) |
> | `normal` | No es contenedor de consulta (por defecto) |

## Paso 2: nombrar el contenedor (opcional)

Con varios contenedores anidados, conviene **nombrarlos** para consultar uno concreto:

```css
.sidebar { container: barra / inline-size; }   /* nombre / tipo */

@container barra (min-width: 20rem) { … }       /* consulta "barra" */
```

El atajo `container: <nombre> / <tipo>` combina `container-name` y `container-type`.

## Paso 3: la regla @container

`@container` funciona como `@media`, pero su condición se evalúa contra el **contenedor de consulta más cercano**:

```css
@container (min-width: 25rem) {
  .tarjeta { /* estilos cuando el contenedor mide ≥ 25rem */ }
}
```

## La consulta es del ancestro, no del propio elemento

> [!warning] Un elemento no se consulta a sí mismo
> Detalle clave: `@container` consulta el tamaño de un **contenedor ancestro**, no del elemento que se estiliza. Por eso el patrón es:
> - El **wrapper** (`.tarjeta-wrap`) lleva `container-type`.
> - El **contenido** (`.tarjeta`) se adapta con `@container`.
>
> Si pones `container-type` en el mismo elemento que quieres estilar según su tamaño, puede crear bucles; por eso se separa el contenedor (padre) del contenido (hijo).

## Ejemplo completo

```css
/* El wrapper es el contenedor de consulta */
.card-wrap { container-type: inline-size; }

/* Por defecto (contenedor estrecho): apilado */
.card { display: grid; gap: 1rem; }

/* Contenedor ancho: imagen y texto en fila */
@container (min-width: 25rem) {
  .card { grid-template-columns: 8rem 1fr; }
}
```
La misma tarjeta se apila en una columna estrecha y se pone en fila en una ancha, **según dónde esté**, sin saber nada del viewport.

## Buenas prácticas

> [!tip] Recomendaciones
> - Marca el **wrapper** con `container-type: inline-size` y adapta el **hijo** con `@container`.
> - Usa `inline-size` (ancho) salvo que necesites también el alto (`size`, requiere altura definida).
> - Nombra los contenedores (`container: nombre / inline-size`) si hay anidación.
> - Combina con media queries: page-level con `@media`, component-level con `@container`.

## Errores comunes

> [!warning] Trampas
> - **`container-type` en el mismo elemento** que quieres estilar según su tamaño (separa padre/hijo).
> - **`size` sin altura definida**: no funciona como se espera.
> - **Olvidar `container-type`**: las reglas `@container` no se aplican sin un contenedor declarado.

## Notas relacionadas

- [[02 Unidades de Contenedor (cqw, cqh)]] — dimensionar respecto al contenedor.
- [[01 Sintaxis @media]] — la sintaxis análoga de las media queries.
- [[index]] — el concepto de container queries.
