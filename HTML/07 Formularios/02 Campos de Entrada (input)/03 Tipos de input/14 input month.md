---
title: input type="month" — Selector de mes y año
aliases:
  - input month
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

# input month

> [!definicion]
> `<input type="month">` permite elegir un **mes y año** concretos, sin día. El valor se almacena como `AAAA-MM`. Es útil para datos donde el día no importa: caducidad de una tarjeta, mes de facturación, periodo de un informe.

```html
<label for="caducidad">Caducidad de la tarjeta</label>
<input type="month" id="caducidad" name="caducidad" min="2026-06" />
```

## Formato y atributos

| Aspecto | Detalle |
|---------|---------|
| Valor | `AAAA-MM` (`2026-06`) |
| `min` / `max` | Acotan el rango de meses |
| `step` | Salto en meses (`step="3"` = trimestral) |

## Casos de uso típicos

| Caso | Por qué month |
|------|---------------|
| Caducidad de tarjeta | Solo importa mes/año |
| Periodo de un informe mensual | "Junio 2026" |
| Mes de inicio de una suscripción | El día es irrelevante |

## Soporte

> [!info] Soporte desigual
> `type="month"` (como `week`) tiene **menos soporte** que `date`: algunos navegadores lo degradan a un campo de texto. Conviene un `placeholder` claro y, si el soporte es crítico, validar el formato con `pattern` como respaldo. Cuando funciona, ofrece un selector cómodo de mes/año.

## Buenas prácticas

> [!tip] Recomendaciones
> - Úsalo cuando el día sea irrelevante (caducidades, periodos mensuales).
> - Acota con `min`/`max` (p. ej. caducidad no anterior al mes actual).
> - Considera un respaldo (`pattern`/placeholder) por el soporte desigual.

## Errores comunes

> [!warning] Trampas
> - **Usar `date` cuando el día no importa**: obliga a elegir un día irrelevante.
> - **No prever el degradado a texto** en navegadores sin soporte.

## Notas relacionadas

- [[12 input date]] — fecha completa con día.
- [[15 input week]] — selección por semana del año.
- [[02 Atributo pattern]] — respaldo de validación de formato.
