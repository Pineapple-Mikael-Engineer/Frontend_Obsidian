---
title: input type="time" — Selector de hora
aliases:
  - input time
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

# input time

> [!definicion]
> `<input type="time">` permite elegir una **hora** (sin fecha). El valor se almacena en formato de 24 horas `hh:mm`, aunque la interfaz puede mostrar AM/PM según la configuración regional del usuario.

```html
<label for="hora">Hora de la reserva</label>
<input type="time" id="hora" name="hora" min="09:00" max="20:00" step="900" />
```

## Formato y atributos

| Aspecto | Detalle |
|---------|---------|
| Valor | `hh:mm` en 24 h (`14:30`); con segundos, `hh:mm:ss` |
| `min` / `max` | Hora más temprana / tardía (útil para horarios) |
| `step` | Granularidad en **segundos**: `step="900"` = cada 15 min |

## step controla los minutos seleccionables

> [!info] step define los intervalos
> Por defecto, `time` salta de minuto en minuto y oculta los segundos. `step` cambia la granularidad, medida en segundos:
> - `step="60"` → cada minuto (por defecto visible).
> - `step="900"` → cada 15 minutos (útil para reservas).
> - `step="1"` → habilita los **segundos** en el control.
>
> Para una agenda de citas en bloques de media hora, `step="1800"`.

## 24 h en el valor, AM/PM en la vista

Igual que [[12 input date | date]], el formato de **almacenamiento** es fijo (24 h, `14:30`) mientras que la **visualización** se adapta a la región (puede mostrar "2:30 PM"). Se procesa siempre el valor de 24 horas.

## Buenas prácticas

> [!tip] Recomendaciones
> - Acota con `min`/`max` para horarios de apertura.
> - Usa `step` acorde a los intervalos reales (15/30 min para reservas).
> - Procesa el valor en 24 h; no asumas el formato visible.

## Errores comunes

> [!warning] Trampas
> - **Asumir AM/PM en el valor**: el valor siempre es 24 h.
> - **No ajustar `step`**: deja elegir minutos que no ofreces (p. ej. 14:07 en reservas por cuartos).

## Notas relacionadas

- [[13 input datetime-local]] — fecha y hora juntas.
- [[12 input date]] — solo fecha.
- [[01 Atributos Comunes de input]] — atributos compartidos.
