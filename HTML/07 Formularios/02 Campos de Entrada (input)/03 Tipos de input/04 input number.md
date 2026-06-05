---
title: input type="number" — Entrada numérica
aliases:
  - input number
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: spinbutton
vacio: true
draft: false
---

# input number

> [!definicion]
> `<input type="number">` acepta solo **valores numéricos**, muestra flechas de incremento/decremento (spinner) y, en móvil, abre el teclado numérico. Respeta los límites `min`, `max` y `step`.

```html
<label for="cantidad">Cantidad</label>
<input type="number" id="cantidad" name="cantidad" min="1" max="99" step="1" value="1" />
```

## min, max y step

| Atributo | Efecto |
|----------|--------|
| `min` | Valor mínimo aceptado |
| `max` | Valor máximo aceptado |
| `step` | Incremento permitido (y granularidad de validación) |

`step` controla qué valores son válidos: con `step="0.5"`, `2.5` es válido pero `2.3` no. Para permitir decimales libres, `step="any"`:

```html
<input type="number" step="0.01" min="0" />   <!-- precios: 2 decimales -->
<input type="number" step="any" />             <!-- cualquier decimal -->
```

## number no es para todo lo que "son números"

> [!warning] Cuándo NO usar number
> `type="number"` es para cantidades sobre las que tiene sentido **sumar o incrementar**: edad, cantidad, precio. **No** para identificadores numéricos que no son magnitudes:
> - Teléfonos → [[05 input tel]] (admiten `+`, espacios; no son cantidades).
> - Códigos postales, tarjetas, OTP → `type="text"` con `inputmode="numeric"` y `pattern`.
>
> El spinner y la validación de `number` no tienen sentido en un número de tarjeta, y pueden recortar ceros a la izquierda.

## inputmode como alternativa

Para "números que no son cantidades", el patrón moderno es `text` + `inputmode="numeric"`, que da el teclado numérico sin la semántica de magnitud:

```html
<input type="text" inputmode="numeric" pattern="[0-9]*" autocomplete="one-time-code" />
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `number` para cantidades reales (con `min`/`max`/`step`).
> - Para teléfonos, códigos o tarjetas, usa `tel`/`text` con `inputmode` y `pattern`.
> - Define `min`/`max` para acotar y `step` para la granularidad.
> - Alinea el campo y su `<label>`; el spinner no sustituye a la etiqueta.

## Errores comunes

> [!warning] Trampas
> - **`number` para teléfonos o códigos**: pierde ceros iniciales, permite `e` (notación científica) y no encaja.
> - **Olvidar `step`** cuando se necesitan decimales: por defecto solo enteros.
> - **No acotar** con `min`/`max` cuando el rango es conocido.

## Notas relacionadas

- [[18 input range]] — el deslizador, alternativa visual para rangos.
- [[05 input tel]] — para teléfonos, no `number`.
- [[03 Restricciones (required, min, max, step, maxlength)]] — `min`/`max`/`step` en detalle.
