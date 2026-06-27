---
title: <meter> — Medida escalar en un rango
aliases:
  - meter element
tags:
  - html
  - api/elemento
  - formularios
elemento: meter
categoria: flujo
rol_implicito: meter
vacio: false
draft: false
order: 2
---

# Medidor (meter)

> [!definicion]
> `<meter>` representa una **medida escalar dentro de un rango conocido**: cuánto espacio queda en disco, el nivel de una batería, la fuerza de una contraseña, una puntuación. No es para progreso de tareas (eso es [[03 Barra de Progreso (progress) | `<progress>`]]); es para una medida que tiene sentido en una escala con mínimo y máximo.

```html
<label for="disco">Espacio usado</label>
<meter id="disco" value="0.7" min="0" max="1"
       low="0.3" high="0.8" optimum="0.2">70 %</meter>
```

## Atributos

| Atributo | Efecto |
|----------|--------|
| `value` | El valor actual de la medida (obligatorio) |
| `min` / `max` | Extremos del rango (por defecto 0 y 1) |
| `low` / `high` | Umbrales que dividen el rango en zonas (bajo / medio / alto) |
| `optimum` | Dónde está el valor "ideal" (cambia qué zona se considera buena) |

## Las zonas y el color

> [!info] optimum decide qué es "bueno"
> Los umbrales `low`/`high` dividen la escala en tres zonas, y `optimum` indica cuál es la deseable. El navegador colorea la barra (verde/amarillo/rojo) según si el valor cae en la zona buena o mala **relativa a `optimum`**:
> - Si `optimum` está en la zona baja (p. ej. uso de disco: cuanto menos, mejor), valores altos se ven "malos" (rojo).
> - Si `optimum` está en la zona alta (p. ej. nivel de batería: cuanto más, mejor), valores bajos se ven "malos".
>
> Así el mismo valor "alto" puede ser bueno o malo según el contexto que define `optimum`.

## El texto de respaldo

El contenido entre las etiquetas (`70 %`) es un **respaldo** para navegadores sin soporte y un texto legible. No es lo que se mide (eso es `value`), sino una representación textual.

## meter vs. progress

| | `<meter>` | `<progress>` |
|--|-----------|--------------|
| Qué representa | Una medida en una escala | El avance de una tarea |
| ¿Se "completa"? | No necesariamente | Sí, llega al 100 % |
| Tiene zonas (low/high/optimum) | Sí | No |
| Ejemplo | Batería, disco, puntuación | Descarga, subida |

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<meter>` para medidas en una escala conocida, no para progreso.
> - Define `low`/`high`/`optimum` para que el color comunique "bueno/malo".
> - Asócialo a un `<label>` y/o pon un texto de respaldo legible.

## Errores comunes

> [!warning] Trampas
> - **Usar `<meter>` para progreso de una tarea**: eso es `<progress>`.
> - **Olvidar `optimum`** cuando "más" puede ser bueno o malo según el caso: el color sale al revés.
> - **Sin `min`/`max` explícitos** cuando el rango no es 0–1.

## Notas relacionadas

- [[03 Barra de Progreso (progress)]] — para el avance de una tarea.
- [[01 Resultado de Cálculo (output)]] — para resultados de cálculos.
