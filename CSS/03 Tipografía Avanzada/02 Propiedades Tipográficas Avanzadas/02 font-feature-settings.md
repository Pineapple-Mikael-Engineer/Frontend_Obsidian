---
title: font-feature-settings — Funciones OpenType de bajo nivel
aliases:
  - font-feature-settings
  - opentype features
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: font-feature-settings
grupo: tipografia
valor_inicial: normal
hereda: true
animable: false
draft: false
---

# font-feature-settings

> [!definicion]
> `font-feature-settings` activa o desactiva **funciones OpenType** de una fuente mediante sus etiquetas de 4 letras: ligaduras (`liga`), versalitas (`smcp`), cifras de estilo antiguo (`onum`), fracciones (`frac`), alternativas estilísticas (`ss01`…) y muchas más. Es el control de **bajo nivel** sobre las capacidades tipográficas de la fuente.

```css
.elegante {
  font-feature-settings: "liga" 1, "dlig" 1, "onum" 1;
}
```

## Sintaxis

```css
font-feature-settings: "etiqueta" valor;
```

| Forma | Significa |
|-------|-----------|
| `"liga" 1` o `"liga" on` | Activa la función |
| `"liga" 0` o `"liga" off` | La desactiva |
| `"ss01"` | Activa (1 implícito) |

## Funciones comunes

| Etiqueta | Función |
|----------|---------|
| `liga` | Ligaduras estándar (fi, fl) |
| `dlig` | Ligaduras discrecionales |
| `smcp` | Versalitas (small caps) |
| `onum` | Cifras de estilo antiguo (con ascendentes/descendentes) |
| `tnum` | Cifras tabulares (ancho fijo) |
| `frac` | Fracciones (1/2 → ½) |
| `kern` | Kerning |
| `ss01`–`ss20` | Conjuntos estilísticos alternativos de la fuente |

## Prefiere las propiedades de alto nivel

> [!warning] font-feature-settings es el último recurso
> Muchas de estas funciones tienen un atajo de **alto nivel** más legible y robusto, que **debe preferirse**:
> | En vez de `font-feature-settings` | Usa |
> |-----------------------------------|-----|
> | `"liga" 1` | [[06 font-variant | `font-variant-ligatures: common-ligatures`]] |
> | `"smcp" 1` | `font-variant-caps: small-caps` |
> | `"onum" 1` | `font-variant-numeric: oldstyle-nums` |
> | `"tnum" 1` | `font-variant-numeric: tabular-nums` |
>
> Las propiedades `font-variant-*` son acumulativas y no se pisan entre sí, mientras que `font-feature-settings` **reemplaza todo** el conjunto cada vez que se declara. Reserva `font-feature-settings` para funciones **sin** atajo (conjuntos estilísticos `ss01`, alternativas específicas de una fuente).

## No es acumulativa

> [!warning] Cada declaración reemplaza la anterior
> Si declaras `font-feature-settings: "liga" 1` y luego, en otra regla, `font-feature-settings: "onum" 1`, la segunda **anula** la primera (las ligaduras se pierden). Hay que listar **todas** las funciones juntas. Esta fragilidad es otra razón para preferir `font-variant-*`, que sí se combinan.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `font-variant-*` (alto nivel) siempre que exista el atajo.
> - Reserva `font-feature-settings` para conjuntos estilísticos (`ssXX`) y alternativas sin atajo.
> - Si lo usas, lista **todas** las funciones en una declaración (no es acumulativa).
> - Verifica qué funciones soporta la fuente (no todas las tienen).

## Errores comunes

> [!warning] Trampas
> - **Declaraciones que se pisan**: la última anula las anteriores.
> - **Usarlo donde hay atajo** `font-variant-*`: menos legible y más frágil.
> - **Activar funciones que la fuente no tiene**: no hacen nada.

## Notas relacionadas

- [[06 font-variant]] — los atajos de alto nivel, preferibles.
- [[03 font-variation-settings]] — el equivalente para fuentes variables (ejes).
- [[01 font-kerning]] — el kerning, una de estas funciones con atajo propio.
