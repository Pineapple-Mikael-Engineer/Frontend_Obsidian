---
title: <button> — Botón con contenido enriquecido
aliases:
  - button element
tags:
  - html
  - api/elemento
  - formularios
elemento: button
categoria: interactivo
rol_implicito: button
vacio: false
draft: false
order: 4
---

# Botones (button)

> [!definicion]
> `<button>` es un botón que, a diferencia de [[03 Tipos de input/20 input submit | `<input type="submit">`]], puede contener **HTML enriquecido**: texto, iconos, varios elementos. Su comportamiento lo define el atributo `type`. Es la forma recomendada de crear botones.

```html
<button type="submit">
  <svg aria-hidden="true">…</svg>
  Guardar cambios
</button>
```

## El atributo type es decisivo

| `type` | Comportamiento |
|--------|----------------|
| `submit` | **Envía el formulario** (es el valor **por defecto** dentro de un form) |
| `reset` | Restablece el formulario |
| `button` | No hace nada por sí solo; acción vía JavaScript |

> [!warning] Sin type, un button envía el formulario
> El error más frecuente con `<button>`: dentro de un `<form>`, un botón **sin `type` explícito** actúa como `type="submit"` y envía el formulario. Un botón de acción (abrir un menú, calcular) **debe** declarar `type="button"`, o provocará envíos y recargas inesperadas:
> ```html
> <!-- ❌ envía el formulario sin querer -->
> <button onclick="abrirMenu()">Menú</button>
>
> <!-- ✅ acción sin enviar -->
> <button type="button" onclick="abrirMenu()">Menú</button>
> ```

## button vs. input vs. a

| Elemento | Para qué |
|----------|----------|
| `<button>` | Una **acción** (enviar, abrir, ejecutar) |
| `<input type="submit/button">` | Acción, pero solo texto (sin HTML dentro) |
| `<a href>` | **Navegar** a otra URL o sección |

> [!info] Botón para acciones, enlace para navegar
> La regla semántica clave: si lleva a otra página o ancla, es un **enlace** ([[01 Enlaces (a) | `<a>`]]); si ejecuta una acción en la página actual (enviar, abrir, borrar), es un **botón**. Usar un `<a href="#">` con JS para una acción, o un `<button>` para navegar, rompe la semántica, la accesibilidad por teclado y el comportamiento esperado (abrir en pestaña nueva, etc.).

## Atributos útiles

| Atributo | Efecto |
|----------|--------|
| `type` | `submit` / `reset` / `button` |
| `name` / `value` | Se envían si el botón hace submit (distinguir qué botón) |
| `disabled` | Deshabilita el botón |
| `form` | Asocia el botón a un `<form>` por su `id`, aunque esté fuera |
| `formaction` / `formmethod` | Sobrescriben el `action`/`method` del form para ese botón |

## Accesibilidad

Un `<button>` es accesible por teclado de forma nativa (foco, Enter/Espacio lo activan) y se anuncia como botón. Si solo lleva un icono, necesita un nombre accesible:

```html
<button type="button" aria-label="Cerrar">
  <svg aria-hidden="true">…</svg>
</button>
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Declara siempre `type` explícito (sobre todo `type="button"` para acciones JS).
> - Usa `<button>` para acciones y `<a>` para navegar.
> - Botón de solo icono: añade `aria-label`.
> - Aprovecha `name`/`value` para distinguir varios botones de envío.

## Errores comunes

> [!warning] Trampas
> - **Olvidar `type`**: envía el formulario sin querer.
> - **`<a>` como botón o `<button>` como enlace**: rompe semántica y accesibilidad.
> - **Botón de icono sin nombre accesible**.
> - **`<div>` con `onclick` como "botón"**: no es accesible por teclado; usa `<button>`.

## Notas relacionadas

- [[03 Tipos de input/20 input submit]] — el botón de envío clásico, menos flexible.
- [[03 Tipos de input/22 input button]] — el botón genérico vía input.
- [[01 Enlaces (a)]] — para navegar, en lugar de un botón.
