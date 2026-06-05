---
title: input type="week" — Selector de semana
aliases:
  - input week
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: ninguno
vacio: true
draft: false
---

# input week

> [!definicion]
> `<input type="week">` permite elegir una **semana** concreta de un año. El valor se almacena como `AAAA-Wnn`, donde `nn` es el número de semana ISO (01–53). Es un tipo de nicho, útil para planificación semanal.

```html
<label for="semana">Semana de entrega</label>
<input type="week" id="semana" name="semana" />
```

## Formato del valor

| Componente | Detalle |
|------------|---------|
| Valor | `AAAA-Wnn` (`2026-W23`) |
| `nn` | Número de semana ISO 8601 (01 a 52/53) |

## La numeración de semanas ISO

> [!info] La semana 1 no es la del 1 de enero
> El estándar ISO 8601 define la semana 1 como la que contiene el **primer jueves** del año (equivalente: la semana que incluye el 4 de enero), y las semanas empiezan en **lunes**. Por eso el 1 de enero puede caer en la última semana del año anterior. Esta convención difiere de otros sistemas (semanas que empiezan en domingo, o numeradas desde el 1 de enero), lo que es fuente de confusión al procesar el valor.

## Soporte limitado

`type="week"` es de los tipos con **menor soporte**: varios navegadores lo muestran como campo de texto. Conviene un respaldo y, a menudo, valorar si una fecha normal ([[12 input date | date]]) o un selector personalizado encaja mejor.

## Buenas prácticas

> [!tip] Recomendaciones
> - Úsalo solo cuando la unidad natural sea la semana (planificación, turnos).
> - Ten presente la convención ISO al interpretar el número de semana.
> - Prevé el degradado a texto por el soporte limitado.

## Errores comunes

> [!warning] Trampas
> - **Asumir que la semana 1 es la del 1 de enero**: la ISO la define por el primer jueves.
> - **Confiar en soporte universal**: es de los tipos menos soportados.

## Notas relacionadas

- [[12 input date]] — fecha por día, más soportada.
- [[14 input month]] — selección por mes, otro tipo de nicho.
