---
title: text-transform — Convertir mayúsculas y minúsculas
aliases:
  - text-transform
  - uppercase
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: text-transform
grupo: tipografia
valor_inicial: none
hereda: true
animable: false
draft: false
order: 10
---

# text-transform

> [!definicion]
> `text-transform` cambia la **capitalización** del texto al mostrarlo, sin alterar el HTML subyacente: convierte a mayúsculas, a minúsculas o capitaliza la inicial de cada palabra. Es presentación pura: el texto real no cambia.

```css
.boton  { text-transform: uppercase; }
.titulo { text-transform: capitalize; }
```

## Valores

| Valor | Efecto | Ejemplo (`hola mundo`) |
|-------|--------|------------------------|
| `none` | Sin cambios | hola mundo |
| `uppercase` | Todo a mayúsculas | HOLA MUNDO |
| `lowercase` | Todo a minúsculas | hola mundo |
| `capitalize` | Primera letra de cada palabra en mayúscula | Hola Mundo |
| `full-width` | Caracteres a ancho completo (CJK) | (uso específico) |

## El texto real no cambia

> [!info] Solo afecta a la visualización
> `text-transform: uppercase` muestra el texto en mayúsculas, pero el HTML sigue teniendo el original. Esto tiene implicaciones útiles:
> - **Se puede seleccionar/copiar** el texto en su forma original.
> - Los **lectores de pantalla** leen el texto real (no deletrean las mayúsculas, salvo siglas).
> - Es **reversible** y no destruye el contenido.
>
> Por eso, para mostrar un título en mayúsculas, es mejor `text-transform: uppercase` que escribir el HTML en mayúsculas: mantiene el contenido legible y accesible.

## capitalize y sus límites

> [!warning] capitalize no entiende de gramática
> `capitalize` pone en mayúscula la **primera letra de cada palabra**, sin criterio gramatical: capitaliza artículos y preposiciones ("El Niño De La Calle") y no respeta siglas ni nombres. Para títulos con reglas de capitalización reales (title case en inglés, mayúscula solo en la primera palabra en español), `capitalize` no sirve: hay que escribir el texto ya capitalizado o usar JS.

## Accesibilidad del uppercase

> [!warning] Mayúsculas con moderación
> Bloques largos de texto en mayúsculas son **más lentos de leer** (se pierden las formas distintivas de las letras) y algunos lectores de pantalla pueden deletrearlos si los confunden con siglas. Reserva `uppercase` para etiquetas cortas (botones, encabezados pequeños), no para párrafos. El `letter-spacing` ligeramente aumentado mejora la legibilidad de las mayúsculas.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `text-transform: uppercase` en lugar de escribir mayúsculas en el HTML: accesible y reversible.
> - Limita las mayúsculas a textos cortos; añade un poco de `letter-spacing`.
> - No confíes en `capitalize` para títulos con reglas gramaticales.
> - Combínalo con [[06 font-variant | versalitas]] cuando quieras un efecto más elegante que el uppercase puro.

## Errores comunes

> [!warning] Trampas
> - **Escribir el HTML en mayúsculas** en vez de usar CSS: pierde el texto original.
> - **`capitalize`** esperando reglas gramaticales: capitaliza todo a ciegas.
> - **Párrafos largos en `uppercase`**: difíciles de leer.

## Notas relacionadas

- [[06 font-variant]] — versalitas, alternativa elegante a `uppercase`.
- [[12 letter-spacing y word-spacing]] — mejorar la legibilidad de mayúsculas.
- [[10 text-transform]] — (esta nota).
