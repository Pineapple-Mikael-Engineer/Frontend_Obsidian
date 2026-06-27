---
title: hyphens — Guionado automático
aliases:
  - hyphens
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: hyphens
grupo: tipografia
valor_inicial: manual
hereda: true
animable: false
draft: false
order: 3
---

# hyphens

> [!definicion]
> `hyphens` controla el **guionado automático**: la partición de palabras al final de la línea con un **guion** (`-`), siguiendo las reglas del idioma. Mejora el aspecto del texto justificado y estrecho, repartiendo mejor el espacio.

```css
p {
  hyphens: auto;
  text-align: justify;
}
```

## Valores

| Valor | Efecto |
|-------|--------|
| `none` | Nunca partir, ni siquiera en guiones suaves |
| `manual` | Solo en puntos marcados por el autor (`&shy;` o `-`) — por defecto |
| `auto` | Partición automática según el diccionario del idioma |

## Requiere lang correcto

> [!warning] hyphens: auto necesita el atributo lang
> El guionado automático depende de las reglas y el diccionario de **cada idioma**, así que el navegador necesita saber el idioma del texto. Sin un [[02 direction | `lang`]] correcto en el HTML, `hyphens: auto` **no funciona** (o usa reglas equivocadas):
> ```html
> <html lang="es">   <!-- imprescindible para que hyphens: auto sepa partir en español -->
> ```
> Es una dependencia que sorprende: el CSS pide guionado, pero el responsable de que funcione es el `lang` del HTML.

## El compañero del justificado

> [!tip] hyphens + justify
> El guionado brilla con [[08 text-align | `text-align: justify`]]: sin guiones, el justificado en columnas estrechas crea "ríos" de espacio feos; con `hyphens: auto`, las palabras se parten y el espacio se reparte de forma más uniforme y legible:
> ```css
> .columna { text-align: justify; hyphens: auto; }
> ```
> Es la combinación clásica para texto justificado de calidad editorial.

## El guion suave manual

Con `hyphens: manual` (por defecto), puedes marcar **dónde** se permite partir una palabra concreta con el guion suave `&shy;` (soft hyphen), invisible salvo que la palabra se rompa ahí:

```html
<p>super&shy;califragilístico</p>   <!-- partirá ahí solo si hace falta -->
```

## Ajustar el guionado

Propiedades relacionadas (`hyphenate-limit-chars`) controlan cuántos caracteres deben quedar antes/después del guion, para evitar particiones feas (un guion tras una sola letra). El soporte varía.

## Buenas prácticas

> [!tip] Recomendaciones
> - `hyphens: auto` con un `lang` correcto, sobre todo en texto justificado o columnas estrechas.
> - Combínalo con `text-align: justify` para un justificado legible.
> - Usa `&shy;` para controlar la partición de palabras concretas.
> - Verifica que el guionado del idioma esté soportado (la mayoría lo están).

## Errores comunes

> [!warning] Trampas
> - **`hyphens: auto` sin `lang`**: no parte (o parte mal).
> - **Justificar sin guionado**: ríos de espacio en columnas estrechas.
> - **Guionado excesivo** en columnas muy anchas: innecesario, distrae.

## Notas relacionadas

- [[08 text-align]] — el justificado que el guionado mejora.
- [[01 overflow-wrap y word-wrap]] — partir sin guion cuando no cabe.
- [[02 Elemento Raíz (html)]] — el `lang` del que depende `hyphens: auto`.
