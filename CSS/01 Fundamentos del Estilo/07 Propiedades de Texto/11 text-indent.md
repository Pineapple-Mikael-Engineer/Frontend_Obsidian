---
title: text-indent — Sangría de la primera línea
aliases:
  - text-indent
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: text-indent
grupo: tipografia
valor_inicial: "0"
hereda: true
animable: true
draft: false
---

# text-indent

> [!definicion]
> `text-indent` aplica una **sangría** a la **primera línea** de un bloque de texto, desplazándola horizontalmente. Es la sangría de párrafo típica de la tipografía impresa.

```css
p { text-indent: 2em; }   /* primera línea sangrada 2em */
```

## Valores

| Valor | Efecto |
|-------|--------|
| Longitud | `2em`, `30px`: desplaza la primera línea esa cantidad |
| Porcentaje | `5%`: relativo al ancho del contenedor |
| Negativo | Sangría "colgante" (la primera línea sale hacia fuera) |

## Solo la primera línea

`text-indent` afecta **únicamente** a la primera línea del bloque, no a las siguientes ni a cada párrafo de un contenedor (salvo que cada uno sea su propio bloque). Para sangrar todos los párrafos, se aplica a cada `<p>` (se hereda, así que basta ponerlo en un contenedor).

## Sangría vs. espacio entre párrafos

> [!info] Dos convenciones de párrafo
> Hay dos formas tradicionales de separar párrafos, y conviene **no mezclarlas**:
> - **Sangría** (`text-indent`) sin espacio entre párrafos: estilo de libro impreso.
> - **Espacio** entre párrafos (`margin`) sin sangría: estilo web habitual.
>
> Usar ambas a la vez (sangría **y** margen) es redundante y se ve recargado. En la web, lo común es margen entre párrafos y `text-indent: 0`; la sangría se reserva para contenido tipo "libro".

## La sangría colgante (negativa)

Un `text-indent` **negativo** saca la primera línea hacia fuera mientras el resto queda sangrado (hanging indent), útil para bibliografías, definiciones o diálogos:

```css
.referencia {
  padding-left: 2em;
  text-indent: -2em;   /* la primera línea "cuelga" hacia la izquierda */
}
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Elige una convención: sangría **o** espacio entre párrafos, no ambas.
> - `text-indent` para estilo editorial (libros, artículos largos).
> - Sangría negativa + `padding` para listas colgantes (bibliografía).
> - Usa `em` para que la sangría escale con el tamaño de fuente.

## Errores comunes

> [!warning] Trampas
> - **Sangría + margen** entre párrafos: redundante y recargado.
> - **Esperar que sangre todas las líneas**: solo la primera.
> - **`%`** sin recordar que es relativo al **ancho** del contenedor.

## Notas relacionadas

- [[08 text-align]] — la alineación, otra propiedad de párrafo.
- [[05 Margin]] — el espacio entre párrafos, alternativa a la sangría.
- [[01 Párrafos (p)]] — el elemento que se sangra.
