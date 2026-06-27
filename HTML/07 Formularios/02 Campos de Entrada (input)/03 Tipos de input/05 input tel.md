---
title: input type="tel" — Número de teléfono
aliases:
  - input tel
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: textbox
vacio: true
draft: false
order: 5
---

# input tel

> [!definicion]
> `<input type="tel">` es para **números de teléfono**. Su aportación principal es el **teclado numérico** en móvil; a diferencia de [[03 input email | email]] o [[06 input url | url]], **no valida** ningún formato, porque los números de teléfono varían demasiado entre países.

```html
<label for="tel">Teléfono</label>
<input type="tel" id="tel" name="tel" autocomplete="tel"
       pattern="[0-9]{9}" placeholder="600123456" />
```

## Por qué no valida

> [!info] La diversidad de formatos
> Un teléfono puede llevar `+`, espacios, guiones, paréntesis, prefijos de país de longitud variable. Imponer un formato único rompería la entrada en muchos países. Por eso `type="tel"` deja la validación al desarrollador, vía [[02 Atributo pattern | `pattern`]], que se ajusta al formato esperado:
> ```html
> <!-- 9 dígitos (España) -->
> <input type="tel" pattern="[0-9]{9}" />
> <!-- Formato internacional con + -->
> <input type="tel" pattern="\+?[0-9 ]{7,15}" />
> ```

## tel vs. number

| | `tel` | `number` |
|--|-------|----------|
| Admite `+`, espacios, guiones | Sí | No |
| Conserva ceros a la izquierda | Sí | No (puede recortarlos) |
| Spinner de incremento | No | Sí |
| Es una "cantidad" | No | Sí |

Un teléfono no es una cantidad: no se suma ni se incrementa. Por eso `tel` (o `text`), nunca `number`.

## autocomplete

`autocomplete="tel"` permite el rellenado automático del teléfono guardado. Hay variantes (`tel-national`, `tel-country-code`) para campos separados.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `tel` para teléfonos: teclado numérico + sin recorte de ceros.
> - Añade `pattern` acorde al formato que aceptes y un `title` explicativo.
> - `autocomplete="tel"` para el rellenado.
> - Indica con `placeholder` el formato esperado.

## Errores comunes

> [!warning] Trampas
> - **Esperar validación automática**: `tel` no valida; añade `pattern`.
> - **Usar `number`**: pierde el `+`, los espacios y los ceros iniciales.
> - **`pattern` demasiado estricto** que rechace formatos internacionales válidos.

## Notas relacionadas

- [[04 input number]] — para cantidades, no teléfonos.
- [[02 Atributo pattern]] — la validación que `tel` necesita.
- [[04 Enlaces Telefónicos (tel)]] — el esquema `tel:` para llamar (distinto contexto).
