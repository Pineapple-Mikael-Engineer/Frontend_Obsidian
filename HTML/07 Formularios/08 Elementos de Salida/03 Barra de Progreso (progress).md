---
title: <progress> — Avance de una tarea
aliases:
  - progress element
tags:
  - html
  - api/elemento
  - formularios
elemento: progress
categoria: flujo
rol_implicito: progressbar
vacio: false
draft: false
---

# Barra de Progreso (progress)

> [!definicion]
> `<progress>` muestra el **avance de una tarea** hacia su finalización: una descarga, una subida de archivo, los pasos completados de un asistente. A diferencia de [[02 Medidor (meter) | `<meter>`]], representa algo que progresa hasta completarse.

```html
<label for="descarga">Descargando…</label>
<progress id="descarga" value="30" max="100">30 %</progress>
```

## Atributos

| Atributo | Efecto |
|----------|--------|
| `value` | Cuánto se ha avanzado (de 0 a `max`) |
| `max` | El valor que representa "completado" (por defecto 1) |

## Determinado vs. indeterminado

> [!info] Sin value, el progreso es "indeterminado"
> Hay dos modos según haya o no `value`:
> - **Con `value`** (determinado): se conoce el porcentaje exacto; la barra se llena proporcionalmente.
> - **Sin `value`** (indeterminado): no se sabe cuánto falta; el navegador muestra una animación de "cargando" sin porcentaje.
> ```html
> <progress value="30" max="100"></progress>   <!-- 30 % -->
> <progress></progress>                          <!-- cargando, indeterminado -->
> ```
> El modo indeterminado es para esperas de duración desconocida (esperando respuesta del servidor).

## Actualización con JavaScript

El progreso real se actualiza por código a medida que avanza la tarea:

```js
// p. ej., progreso de una subida
xhr.upload.onprogress = (e) => {
  barra.value = e.loaded;
  barra.max = e.total;
};
```

## progress vs. meter

| | `<progress>` | `<meter>` |
|--|--------------|-----------|
| Representa | Avance hacia una meta | Una medida en una escala |
| Llega al 100 % | Sí | No necesariamente |
| Tiene umbrales de color | No | Sí (`low`/`high`/`optimum`) |
| Ejemplo | Descarga, pasos de un wizard | Batería, disco, puntuación |

## Accesibilidad

Su rol `progressbar` hace que los lectores de pantalla anuncien el avance. Conviene acompañarlo de un `<label>` o texto que diga **qué** progresa ("Subiendo archivo: 30 %"), no solo la barra.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<progress>` para tareas que avanzan hacia completarse.
> - Omite `value` (indeterminado) cuando no conozcas la duración.
> - Acompáñalo de texto que indique qué tarea y, si es posible, el porcentaje.
> - Para una medida estática en una escala, usa `<meter>`.

## Errores comunes

> [!warning] Trampas
> - **Usar `<progress>` para una medida estática**: eso es `<meter>`.
> - **Olvidar actualizar `value`**: la barra se queda congelada.
> - **Barra sin contexto textual**: el usuario no sabe qué progresa.

## Notas relacionadas

- [[02 Medidor (meter)]] — para medidas en una escala, no progreso.
- [[01 Resultado de Cálculo (output)]] — otro elemento de salida.
