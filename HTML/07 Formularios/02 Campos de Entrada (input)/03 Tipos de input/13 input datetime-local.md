---
title: input type="datetime-local" — Fecha y hora locales
aliases:
  - input datetime-local
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

# input datetime-local

> [!definicion]
> `<input type="datetime-local">` permite elegir **fecha y hora** juntas, sin información de zona horaria (de ahí "local"). El valor se almacena como `AAAA-MM-DDThh:mm` (con una `T` que separa fecha y hora).

```html
<label for="cita">Fecha y hora de la cita</label>
<input type="datetime-local" id="cita" name="cita" min="2026-06-05T09:00" />
```

## Formato del valor

| Componente | Formato |
|------------|---------|
| Fecha + hora | `2026-06-04T14:30` |
| Con segundos (si `step` lo permite) | `2026-06-04T14:30:00` |

La `T` mayúscula entre la fecha y la hora es parte del formato ISO 8601 y es obligatoria en el valor.

## El problema de la zona horaria

> [!warning] datetime-local no guarda zona horaria
> El "local" significa que el valor es la hora **tal cual la introdujo el usuario**, sin zona. Si una cita es "14:30", no se sabe si son las 14:30 de Madrid o de México. Para eventos que cruzan zonas horarias, el servidor debe:
> - Conocer la zona del usuario (por otro campo, o por JavaScript con `Intl`).
> - Convertir y almacenar en **UTC**, mostrando luego en la zona de cada usuario.
>
> Usar `datetime-local` tal cual solo es seguro cuando todos los usuarios están en la misma zona o la zona es irrelevante.

## min, max, step

Igual que [[12 input date | date]]: `min`/`max` acotan el rango (en formato con `T`), y `step` controla la granularidad (en segundos; `step="60"` = minutos, el valor por defecto omite segundos).

## Buenas prácticas

> [!tip] Recomendaciones
> - Úsalo cuando necesites fecha y hora a la vez en la zona del usuario.
> - Para eventos globales, captura también la zona horaria y normaliza a UTC en servidor.
> - Acota con `min`/`max` (p. ej. solo horarios futuros y laborables).

## Errores comunes

> [!warning] Trampas
> - **Ignorar la zona horaria**: una cita "14:30" sin zona es ambigua entre usuarios.
> - **Olvidar la `T`** al construir el valor por código.
> - **Confundirlo con date**: este incluye la hora.

## Notas relacionadas

- [[12 input date]] — solo fecha, sin hora.
- [[16 input time]] — solo hora, sin fecha.
- [[01 Atributos Comunes de input]] — atributos compartidos.
