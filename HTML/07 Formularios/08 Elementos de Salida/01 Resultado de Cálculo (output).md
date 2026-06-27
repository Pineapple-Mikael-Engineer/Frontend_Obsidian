---
title: <output> — Resultado de un cálculo
aliases:
  - output element
tags:
  - html
  - api/elemento
  - formularios
elemento: output
categoria: interactivo
rol_implicito: status
vacio: false
draft: false
order: 1
---

# Resultado de Cálculo (output)

> [!definicion]
> `<output>` muestra el **resultado de un cálculo** realizado en el cliente, normalmente a partir de otros controles del formulario. Su rol de accesibilidad es `status`, lo que hace que los lectores de pantalla **anuncien** sus cambios automáticamente (live region).

```html
<input type="range" id="precio" min="0" max="100" value="50"
       oninput="total.value = precio.value + ' €'" />
<output id="total" name="total">50 €</output>
```

## Atributos

| Atributo | Efecto |
|----------|--------|
| `for` | `id`(s) de los controles de los que depende el cálculo (separados por espacios) |
| `name` | Nombre (se puede incluir en el envío) |

El atributo `for` documenta semánticamente qué entradas alimentan el resultado:

```html
<output for="cantidad precio">Total: 30 €</output>
```

## El valor va entre las etiquetas

Como [[03 Área de Texto (textarea) | `<textarea>`]], el contenido de `<output>` se escribe **entre las etiquetas**, no en `value` del HTML (aunque por JS se actualiza con la propiedad `.value`):

```html
<output>0</output>   <!-- valor inicial entre etiquetas -->
```

## Es una región viva (live region)

> [!info] Anuncia sus cambios solo
> El rol `status` de `<output>` lo convierte en una **live region** `aria-live="polite"` implícita: cuando su contenido cambia, los lectores de pantalla lo **anuncian** sin que el usuario tenga que ir a buscarlo. Por eso es la forma semánticamente correcta de mostrar un resultado que se recalcula (el total de un carrito, el resultado de un slider), mejor que un `<span>` que el lector no anunciaría al cambiar.

## Ejemplo: suma de dos campos

```html
<form oninput="suma.value = (+a.value) + (+b.value)">
  <input type="number" id="a" value="0" /> +
  <input type="number" id="b" value="0" /> =
  <output id="suma" name="suma">0</output>
</form>
```

El `oninput` en el `<form>` recalcula cada vez que cambia cualquier campo.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<output>` para resultados calculados que cambian, no un `<span>`.
> - Indica las dependencias con `for`.
> - Aprovecha que anuncia cambios para mejorar la accesibilidad de cálculos en vivo.

## Errores comunes

> [!warning] Trampas
> - **Usar un `<span>`/`<div>`** para resultados dinámicos: el lector no anuncia su cambio.
> - **Poner el valor en un atributo `value`** del HTML: va entre las etiquetas.

## Notas relacionadas

- [[03 Tipos de input/18 input range]] — el slider cuyo valor suele mostrarse en `<output>`.
- [[03 Barra de Progreso (progress)]] · [[02 Medidor (meter)]] — los otros elementos de salida.
