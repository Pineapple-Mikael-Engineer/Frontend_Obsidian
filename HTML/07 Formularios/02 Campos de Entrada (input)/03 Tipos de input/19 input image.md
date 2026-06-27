---
title: input type="image" — Botón de envío con imagen
aliases:
  - input image
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: button
vacio: true
draft: false
order: 19
---

# input image

> [!definicion]
> `<input type="image">` es un **botón de envío** que usa una imagen en lugar de un botón estándar. Al hacer clic, envía el formulario y, además, transmite las **coordenadas** del punto donde se pulsó dentro de la imagen.

```html
<input type="image" src="enviar.png" alt="Enviar formulario" width="120" />
```

## Atributos propios

| Atributo | Efecto |
|----------|--------|
| `src` | La imagen del botón |
| `alt` | Texto alternativo (**obligatorio**: es un control, no decorativo) |
| `width` / `height` | Dimensiones |
| `name` | Si se indica, envía `name.x` y `name.y` con las coordenadas del clic |

## Las coordenadas del clic

> [!info] Un vestigio de los image maps
> El rasgo peculiar de `type="image"` es que envía la posición del clic: con `name="pos"`, el formulario incluye `pos.x=34&pos.y=12`. Esto venía de los mapas de imagen del servidor de los años 90. Hoy rara vez se necesita; casi siempre se usa solo como "botón bonito de envío".

## Por qué casi nunca conviene

> [!warning] Prefiere button con una imagen dentro
> Para un botón de envío con imagen, [[06 Botones (button) | `<button type="submit">`]] es casi siempre mejor: puede contener una `<img>` **y** texto, es más flexible de estilar y más accesible. `type="image"` se reserva para el caso raro en que de verdad se necesitan las coordenadas del clic.
> ```html
> <!-- Preferible -->
> <button type="submit"><img src="enviar.png" alt="" /> Enviar</button>
> ```

## Accesibilidad

El `alt` es **obligatorio** y debe describir la **acción** ("Enviar formulario", "Buscar"), no la imagen, porque funciona como botón. Sin `alt`, un lector de pantalla anuncia un botón sin nombre.

## Buenas prácticas

> [!tip] Recomendaciones
> - En general, usa `<button type="submit">` con una `<img>` dentro, no `type="image"`.
> - Si lo usas, el `alt` describe la acción, no la imagen.
> - Solo tiene sentido cuando necesitas las coordenadas del clic.

## Errores comunes

> [!warning] Trampas
> - **`alt` ausente o que describe la imagen**: el botón queda sin nombre accesible.
> - **Usarlo por estética** cuando `<button>` haría el trabajo mejor.

## Notas relacionadas

- [[06 Botones (button)]] — la alternativa preferida para botones con imagen.
- [[20 input submit]] — el botón de envío estándar.
- [[03 Mapa de Imagen (map, area)]] — los image maps, de donde vienen las coordenadas.
