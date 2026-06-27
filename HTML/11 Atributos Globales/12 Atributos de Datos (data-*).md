---
title: data-* — Atributos de datos personalizados
aliases:
  - data attributes
  - dataset
tags:
  - html
  - api/atributo
  - atributos
elemento: global
categoria: ninguna
rol_implicito: ninguno
vacio: false
draft: false
order: 12
---

# Atributos de Datos (data-*)

> [!definicion]
> Los atributos `data-*` permiten almacenar **datos personalizados** en cualquier elemento HTML, para que JavaScript los lea. Son el puente estándar entre el marcado y el comportamiento: guardan información ("este producto es el id 42", "este botón abre el modal X") sin recurrir a clases o `id` semánticamente impropios.

```html
<button data-producto-id="42" data-accion="añadir-carrito">Comprar</button>
<li data-precio="29.99" data-stock="12">Camiseta</li>
```

## La sintaxis: data- + nombre

Cualquier atributo que empiece por `data-` es válido. El nombre tras `data-` es libre (en minúsculas, con guiones):

```html
<div data-usuario-rol="admin" data-tema="oscuro">…</div>
```

## Leerlos desde JavaScript: dataset

> [!info] data-* se lee con la propiedad dataset
> JavaScript accede a estos atributos a través de la propiedad `dataset`, que convierte `data-nombre-compuesto` a `camelCase`:
> ```html
> <article data-producto-id="42" data-en-oferta="true">…</article>
> ```
> ```js
> el.dataset.productoId   // "42"   (data-producto-id → productoId)
> el.dataset.enOferta     // "true" (data-en-oferta → enOferta)
> ```
> El guion-minúscula del HTML se vuelve camelCase en JS. El detalle de `dataset` pertenece al curso de JavaScript.

## También accesibles desde CSS

CSS puede leerlos con selectores de atributo y mostrarlos con `attr()`:

```css
[data-estado="activo"] { border-color: green; }
.etiqueta::after { content: attr(data-precio) " €"; }
```

## Cuándo usarlos (y cuándo no)

| Usar `data-*` | No usar `data-*` |
|---------------|------------------|
| Datos para JS sobre un elemento | Datos que deben **enviarse** en un formulario (usa `<input type="hidden">`) |
| Estado de UI (`data-abierto`) | Contenido que el usuario debe ver (ponlo como contenido real) |
| Configuración de un componente | Grandes volúmenes de datos (usa una API/JSON) |

> [!warning] data-* es para JS, no para enviar al servidor
> A diferencia de [[08 input hidden | `<input type="hidden">`]] (que viaja con el formulario), los `data-*` **no se envían** al servidor en un submit: solo existen en el cliente para JavaScript y CSS. Si el dato debe llegar al servidor con el formulario, usa un campo hidden.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `data-*` para datos de cliente que JS necesita, en vez de clases/`id` impropios.
> - Nombres descriptivos en minúscula con guiones (`data-producto-id`).
> - Para datos a enviar en un formulario, usa `<input type="hidden">`.
> - No metas en el DOM grandes cantidades de datos: para eso, una API.

## Errores comunes

> [!warning] Trampas
> - **Esperar que se envíen** en un submit: no lo hacen.
> - **Abusar de clases** (`class="precio-29-99"`) para guardar datos en vez de `data-*`.
> - **Datos sensibles** en `data-*`: son visibles en el HTML.

## Notas relacionadas

- [[08 input hidden]] — para datos que **sí** deben enviarse con el formulario.
- [[01 Identificación (id, class)]] — no usar clases/id para guardar datos.
- [[05 dataset]] — leer `data-*` desde JavaScript (delegado a JS).
