---
title: id y class — Identificar y clasificar elementos
aliases:
  - id
  - class
tags:
  - html
  - api/atributo
  - atributos
elemento: global
categoria: ninguna
rol_implicito: ninguno
vacio: false
draft: false
---

# Identificación (id, class)

> [!definicion]
> `id` y `class` identifican elementos para enlazarlos con CSS y JavaScript. `id` es un identificador **único** (un solo elemento por página); `class` es una etiqueta **reutilizable** (muchos elementos pueden compartirla, y uno puede tener varias). Son los atributos globales más usados.

```html
<div id="cabecera" class="caja destacada">…</div>
```

## id vs. class

| | `id` | `class` |
|--|------|---------|
| Unicidad | **Único** en la página | Reutilizable |
| Cuántos por elemento | Uno | Varios (separados por espacios) |
| Selector CSS | `#id` | `.class` |
| Especificidad CSS | Alta | Media |
| Uso típico | Anclas, JS puntual, `for`/`aria` | Estilado, agrupar |

```html
<article id="post-42" class="post destacado nuevo">…</article>
<!-- un id único; tres clases -->
```

## Para qué sirve cada uno

### id

- **Anclas**: destino de [[02 Anclas Internas (id) | enlaces `#id`]].
- **Asociación**: `<label for="id">`, `aria-labelledby="id"`, `aria-describedby="id"`.
- **JavaScript puntual**: `document.getElementById("id")`.

### class

- **Estilado**: el gancho preferido de CSS (`.boton`, `.destacado`).
- **Agrupar**: aplicar el mismo estilo o comportamiento a muchos elementos.
- **Estados**: alternar clases con JS (`.activo`, `.oculto`).

## Reglas del id

> [!warning] Un id, una vez
> Un `id` debe ser **único** en toda la página. Repetirlo:
> - Hace que `getElementById` devuelva solo el **primero**.
> - Rompe las anclas (`#id` salta solo al primero).
> - Es HTML inválido.
>
> Además, no debe llevar espacios (usa guiones: `mi-caja`) y distingue mayúsculas (`#Top` ≠ `#top`).

## Preferir class para estilar

> [!tip] CSS por clase, no por id
> Aunque `#id` funcione como selector CSS, la práctica recomendada es estilar con **clases**, no con `id`:
> - Las clases son **reutilizables**; los `id`, no.
> - Los `id` tienen **especificidad alta**, lo que provoca guerras de especificidad difíciles de mantener.
>
> Reserva el `id` para anclas, asociación de etiquetas y JS puntual; usa `class` para el estilo. El detalle de la especificidad pertenece al curso de CSS.

## Buenas prácticas

> [!tip] Recomendaciones
> - `id` único y descriptivo; sin espacios; en minúsculas con guiones.
> - Estila con `class`, no con `id`.
> - Usa `id` para anclas, `for`/`aria-*` y JS que apunta a un único elemento.
> - Nombra las clases por su **función** o componente, no por su apariencia (`.alerta`, no `.rojo`).

## Errores comunes

> [!warning] Trampas
> - **`id` duplicado**: rompe JS, anclas y validez.
> - **Estilar todo por `id`**: guerras de especificidad.
> - **`id`/`class` con espacios** o caracteres conflictivos.

## Notas relacionadas

- [[02 Anclas Internas (id)]] — el `id` como destino de enlaces.
- [[12 Atributos de Datos (data-*)]] — para datos, en vez de abusar de clases/id.
- [[01 Etiqueta de Campo (label)]] — `for` referencia un `id`.
