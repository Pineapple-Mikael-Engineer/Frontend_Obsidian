---
title: input type="search" — Campo de búsqueda
aliases:
  - input search
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: searchbox
vacio: true
draft: false
---

# input search

> [!definicion]
> `<input type="search">` es un campo de texto pensado para **búsquedas**. Funcionalmente es como [[01 input text | text]], pero el navegador suele añadir una **"x" para borrar** el contenido y, en móvil, una tecla "Buscar" en el teclado. Su rol de accesibilidad es `searchbox`.

```html
<form role="search" action="/buscar">
  <label for="q">Buscar</label>
  <input type="search" id="q" name="q" placeholder="Buscar productos…" />
</form>
```

## Qué aporta sobre text

| Característica | Efecto |
|---------------|--------|
| Botón de borrado (x) | Limpia el campo con un clic (según navegador) |
| Tecla "Buscar" en móvil | El teclado muestra acción de búsqueda en lugar de "Enter" |
| Rol `searchbox` | La asistencia lo anuncia como campo de búsqueda |
| Historial de búsquedas | Algunos navegadores recuerdan búsquedas previas |

## El contexto: form role="search"

Para que la búsqueda sea un landmark navegable, el `<form>` que la contiene lleva `role="search"`. Eso permite a los lectores de pantalla saltar directamente al buscador:

```html
<form role="search">
  <input type="search" name="q" aria-label="Buscar en el sitio" />
  <button type="submit">Buscar</button>
</form>
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `type="search"` para buscadores: mejor UX (botón de borrado, tecla de búsqueda).
> - Envuélvelo en un `<form role="search">` para el landmark de accesibilidad.
> - Etiquétalo con `<label>` o `aria-label`.
> - El estilo del botón "x" varía entre navegadores; normaliza con CSS si lo necesitas.

## Errores comunes

> [!warning] Trampas
> - **Usar `text` para el buscador**: pierdes el botón de borrado y la tecla de búsqueda.
> - **Buscador sin etiqueta**: un icono de lupa solo no basta; añade `aria-label`.
> - **Olvidar `role="search"`** en el formulario: pierde el landmark.

## Notas relacionadas

- [[01 input text]] — el campo base del que deriva.
- [[03 Navegación (nav)]] — los landmarks de navegación, emparentados con `role="search"`.
- [[01 Atributos Comunes de input]] — atributos compartidos.
