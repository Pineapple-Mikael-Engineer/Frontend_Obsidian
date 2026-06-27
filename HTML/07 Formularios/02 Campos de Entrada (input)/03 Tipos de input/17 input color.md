---
title: input type="color" — Selector de color
aliases:
  - input color
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: ninguno
vacio: true
draft: false
order: 17
---

# input color

> [!definicion]
> `<input type="color">` muestra un **selector de color** nativo (una muestra que abre la paleta del sistema). El valor es siempre un color hexadecimal válido en formato `#rrggbb`, en minúsculas.

```html
<label for="tema">Color del tema</label>
<input type="color" id="tema" name="tema" value="#cba6f7" />
```

## El valor siempre es válido

> [!info] Nunca un color inválido
> A diferencia de un campo de texto donde el usuario podría escribir `azul` o `#zzz`, `type="color"` **siempre** devuelve un hex de 6 dígitos válido (`#rrggbb`). Su valor por defecto, si no se indica, es `#000000` (negro). Esto simplifica el procesamiento: no hace falta validar el formato.

## Limitaciones del formato

| Limitación | Detalle |
|------------|---------|
| Solo `#rrggbb` | No admite `rgba()`, `hsl()` ni transparencia (alfa) |
| Sin alfa | Para opacidad, hace falta un control aparte o una librería |
| Sin nombres | No devuelve `red`, siempre el hex |

Si necesitas transparencia o espacios de color avanzados, se recurre a un selector personalizado en JavaScript.

## Sincronizar con un campo de texto

Un patrón habitual es mostrar el hex en un campo de texto editable junto al selector, sincronizados con JS, para que el usuario pueda pegar un código o usar la paleta indistintamente:

```html
<input type="color" id="picker" value="#cba6f7" />
<input type="text" id="hex" value="#cba6f7" pattern="#[0-9a-fA-F]{6}" />
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Da un `value` inicial coherente (no dejar el negro por defecto si no aplica).
> - Para mostrar/editar el hex, sincroniza con un campo de texto.
> - Si necesitas alfa o `hsl`, usa una librería de color; el nativo solo da `#rrggbb`.

## Errores comunes

> [!warning] Trampas
> - **Esperar transparencia**: `color` no soporta alfa.
> - **Asumir otro formato**: el valor siempre es `#rrggbb` en minúsculas.

## Notas relacionadas

- [[01 Atributos Comunes de input]] — `value` y atributos compartidos.
- [[02 Validación Específica por Tipo]] — tipos que siempre devuelven un valor válido.
