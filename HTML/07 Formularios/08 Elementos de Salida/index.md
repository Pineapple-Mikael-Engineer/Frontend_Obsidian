---
title: Elementos de Salida — output, meter, progress
aliases:
  - output elements
  - salida de formulario
tags:
  - html
  - api/concepto
  - formularios
draft: false
---

# Elementos de Salida

> [!definicion]
> Frente a los controles que **reciben** datos del usuario, estos tres elementos **muestran** información calculada o medida: [[01 Resultado de Cálculo (output) | `<output>`]] presenta el resultado de un cálculo, [[02 Medidor (meter) | `<meter>`]] una medida dentro de un rango conocido, y [[03 Barra de Progreso (progress) | `<progress>`]] el avance de una tarea.

```html
<output>42</output>
<meter value="0.7" min="0" max="1">70 %</meter>
<progress value="30" max="100">30 %</progress>
```

## Los tres comparados

| Elemento | Muestra | Ejemplo |
|----------|---------|---------|
| `<output>` | Resultado de un cálculo en cliente | Subtotal de un carrito |
| `<meter>` | Una medida en un rango con significado | Espacio en disco, nivel de batería |
| `<progress>` | Avance hacia una meta | Descarga, subida, pasos completados |

## meter vs. progress: la confusión clásica

> [!info] Medida estática vs. progreso hacia una meta
> Se confunden porque ambos son "barras":
> - `<meter>` mide algo que **tiene un valor en un rango conocido** y no necesariamente avanza: cuánto disco queda, la puntuación de una contraseña, la temperatura. Tiene zonas (bajo/óptimo/alto).
> - `<progress>` mide el **avance de una tarea** hacia su finalización: una descarga al 30 %, 3 de 5 pasos. Va de 0 a completado.
>
> Regla: si "se completa", es `<progress>`; si "es una medida en una escala", es `<meter>`.

## Mapa de la sección

- [[01 Resultado de Cálculo (output)]] — mostrar resultados de cálculos en cliente.
- [[02 Medidor (meter)]] — una medida escalar con rango y umbrales.
- [[03 Barra de Progreso (progress)]] — el avance de una tarea.

## Notas relacionadas

- [[03 Tipos de input/18 input range]] — un deslizador cuyo valor suele mostrarse con `<output>`.
- [[02 Campos de Entrada (input)/index]] — los controles de entrada, contraparte de estos de salida.
