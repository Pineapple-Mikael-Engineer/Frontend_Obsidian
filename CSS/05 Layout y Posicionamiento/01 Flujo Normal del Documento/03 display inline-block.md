---
title: display inline-block — Híbrido en línea con dimensiones
aliases:
  - display inline-block
tags:
  - css
  - api/propiedad
  - layout
propiedad: display
grupo: layout
valor_inicial: inline
hereda: false
animable: false
draft: false
order: 3
---

# display inline-block

> [!definicion]
> `display: inline-block` es un **híbrido**: el elemento fluye **en línea** (junto a otros, sin saltar de línea) como un `inline`, pero **acepta `width`, `height` y márgenes/paddings en todos los lados** como un `block`. Combina lo mejor de ambos.

```css
.chip { display: inline-block; padding: 0.25rem 0.75rem; }
```

## Lo que aporta sobre inline

| Aspecto | `inline` | `inline-block` |
|---------|----------|----------------|
| Fluye en línea | Sí | Sí |
| Respeta `width`/`height` | **No** | **Sí** |
| Padding/margin vertical empuja | No | **Sí** |
| Salta de línea | No | No |

## Casos de uso

```css
/* Etiquetas/chips que fluyen pero tienen tamaño y padding */
.tag {
  display: inline-block;
  padding: 0.25rem 0.75rem;
  background: #cba6f7;
  border-radius: 9999px;
}

/* Botones en línea con dimensiones */
.btn { display: inline-block; min-width: 8rem; text-align: center; }

/* Iconos cuadrados junto a texto */
.icono { display: inline-block; width: 1.5rem; height: 1.5rem; }
```

## El histórico: layout antes de Flexbox

> [!info] inline-block era la herramienta de layout antigua
> Antes de Flexbox y Grid, `inline-block` se usaba mucho para **colocar cajas en fila** (columnas, galerías, menús horizontales), porque permitía cajas con tamaño una al lado de otra. Funcionaba, pero tenía dos lacras: el **hueco fantasma** del espacio en blanco entre elementos, y la dificultad de alinear y repartir el espacio. Hoy, para colocar cajas en fila, **Flexbox** es muy superior. `inline-block` queda para casos puntuales (chips, botones inline, iconos).

## El problema del espacio en blanco

> [!warning] El hueco fantasma entre inline-blocks
> Como los `inline-block` son elementos en línea, el espacio/salto de línea del HTML entre ellos se renderiza como un **hueco** de unos píxeles:
> ```html
> <span class="ib">A</span>
> <span class="ib">B</span>   <!-- hay un hueco entre A y B -->
> ```
> Trucos para eliminarlo (comentarios HTML, `font-size: 0` en el padre) son frágiles. **Flexbox no tiene este problema**: es otra razón para preferirlo al maquetar filas.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `inline-block` para elementos que fluyen con el texto pero necesitan tamaño/padding (chips, botones, iconos).
> - Para **layout de filas/columnas**, usa Flexbox (sin huecos fantasma, con alineación real).
> - Controla la alineación vertical de los inline-block con [[03 Alineación Vertical (vertical-align) | `vertical-align`]].
> - Ten presente el hueco del espacio en blanco si los alineas.

## Errores comunes

> [!warning] Trampas
> - **Maquetar columnas con `inline-block`** en vez de Flexbox/Grid: huecos y alineación frágil.
> - **Olvidar el hueco fantasma** entre elementos.
> - **Desalineación vertical** sin ajustar `vertical-align`.

## Notas relacionadas

- [[01 display block]] · [[02 display inline]] — los dos comportamientos que combina.
- [[01 Contenedor Flex (display flex)]] — la alternativa moderna para filas.
- [[03 Alineación Vertical (vertical-align)]] — alinear inline-blocks verticalmente.
