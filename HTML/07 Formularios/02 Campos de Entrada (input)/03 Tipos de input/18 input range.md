---
title: input type="range" — Deslizador
aliases:
  - input range
  - slider
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: slider
vacio: true
draft: false
---

# input range

> [!definicion]
> `<input type="range">` es un **deslizador** (slider) para elegir un número dentro de un rango arrastrando un control. Es la alternativa visual a [[04 input number | number]] cuando importa más la posición relativa que el valor exacto: volumen, brillo, precio aproximado.

```html
<label for="vol">Volumen</label>
<input type="range" id="vol" name="vol" min="0" max="100" step="1" value="50" />
```

## Atributos

| Atributo | Efecto |
|----------|--------|
| `min` / `max` | Extremos del rango (por defecto 0 y 100) |
| `step` | Incremento del deslizador |
| `value` | Posición inicial |
| `list` | Asocia un `<datalist>` para marcar puntos (ticks) |

## El valor exacto no se ve

> [!warning] range oculta el número
> El deslizador **no muestra el valor numérico** por defecto: el usuario ve la posición, no la cifra. Por eso `range` es inadecuado cuando el valor exacto importa (una edad, un importe que debe ser preciso). Si necesitas que se vea, muéstralo aparte con [[01 Resultado de Cálculo (output) | `<output>`]] y JavaScript:
> ```html
> <input type="range" id="vol" min="0" max="100" value="50"
>        oninput="salida.value = this.value" />
> <output id="salida">50</output>
> ```

## Cuándo range y cuándo number

| Usa `range` | Usa `number` |
|-------------|--------------|
| La posición relativa importa más que la cifra | El valor exacto es esencial |
| Ajuste continuo (volumen, brillo) | Cantidad concreta (edad, unidades) |
| Rango acotado y conocido | Entrada precisa |

## Marcas con datalist

Asociar un `<datalist>` con el atributo `list` dibuja marcas (ticks) en posiciones concretas del deslizador:

```html
<input type="range" min="0" max="100" step="25" list="marcas" />
<datalist id="marcas">
  <option value="0"></option><option value="50"></option><option value="100"></option>
</datalist>
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `range` para ajustes donde la posición importa más que la cifra exacta.
> - Muestra el valor con `<output>` si el usuario necesita verlo.
> - Define `min`/`max`/`step` coherentes con el dominio.
> - Para un valor preciso e importante, usa `number`.

## Errores comunes

> [!warning] Trampas
> - **Usar `range` para valores exactos** sin mostrar la cifra.
> - **No mostrar el valor** cuando el usuario necesita conocerlo.
> - **Rango/`step` mal definidos** que impiden alcanzar valores deseados.

## Notas relacionadas

- [[04 input number]] — para valores exactos.
- [[01 Resultado de Cálculo (output)]] — mostrar el valor del deslizador.
- [[05 Lista de Datos (datalist)]] — marcas/ticks en el deslizador.
