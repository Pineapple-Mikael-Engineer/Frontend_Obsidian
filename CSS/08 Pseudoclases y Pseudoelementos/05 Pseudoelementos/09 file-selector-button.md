---
title: file-selector-button — El botón de los inputs de archivo
aliases:
  - "::file-selector-button"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: "::file-selector-button"
grupo: pseudoelemento
draft: false
order: 9
---

# ::file-selector-button

> [!definicion]
> `::file-selector-button` estiliza el **botón** de un `<input type="file">`: el "Elegir archivo" / "Examinar" que abre el selector de archivos. Permite personalizar ese botón nativo, que históricamente era casi imposible de estilar.

```css
input[type="file"]::file-selector-button {
  background: #cba6f7;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 6px;
  cursor: pointer;
}
```

## El input de archivo, por fin estilable

> [!tip] Personalizar el botón nativo de subida
> El `<input type="file">` tiene dos partes: el **botón** y el **texto** ("Sin archivos seleccionados"). `::file-selector-button` da acceso al botón, que antes solo se podía personalizar con trucos (ocultar el input y usar un `<label>` falso):
> ```css
> input[type="file"]::file-selector-button {
>   background: #cba6f7;
>   color: white;
>   border: none;
>   padding: 0.5rem 1rem;
>   border-radius: 6px;
>   font-weight: 500;
>   margin-right: 1rem;
>   cursor: pointer;
>   transition: background 0.2s;
> }
> input[type="file"]::file-selector-button:hover {
>   background: #b48ef0;
> }
> ```

## Solo el botón, no el texto

> [!warning] El texto del archivo no se puede estilar
> `::file-selector-button` solo alcanza el **botón**, no el texto que muestra el nombre del archivo seleccionado (o "Sin archivos seleccionados"). Ese texto no es estilable directamente. Si necesitas control total del aspecto (incluido el texto), aún se recurre al truco clásico: ocultar el input y usar un `<label>` estilizado más JavaScript para mostrar el nombre.

## El truco clásico, cuando no basta

> [!info] Ocultar el input y usar un label
> Cuando `::file-selector-button` no da suficiente control (porque quieres rediseñar el texto, mostrar una vista previa, etc.), el patrón tradicional sigue vigente:
> ```html
> <label class="boton-subida">
>   Subir foto
>   <input type="file" hidden>
> </label>
> ```
> ```css
> .boton-subida { /* estilado completo como un botón */ }
> ```
> El `<input hidden>` mantiene la funcionalidad; el `<label>` da el aspecto. Con JS se muestra el nombre del archivo elegido. `::file-selector-button` es más simple cuando basta con estilar el botón nativo.

## El prefijo histórico

> [!info] -webkit-file-upload-button
> Antes del estándar `::file-selector-button`, los navegadores WebKit usaban `::-webkit-file-upload-button`. Hoy el estándar tiene buen soporte, pero en código antiguo puede verse el prefijo.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `::file-selector-button` para personalizar el botón nativo de subida (lo más simple).
> - Para control total (texto, vista previa), usa el truco de `<label>` + `<input hidden>` + JS.
> - Da feedback de `:hover`/`:focus` al botón como a cualquier otro.
> - Recuerda que el texto del nombre de archivo no es estilable.

## Errores comunes

> [!warning] Trampas
> - **Esperar estilar el texto** del archivo: solo el botón es accesible.
> - **Ocultar el input con `display: none`** sin un label que dispare el selector: rompe la funcionalidad.
> - **Olvidar el respaldo** `::-webkit-file-upload-button` en navegadores antiguos.

## Notas relacionadas

- [[02 Etiquetas (label)]] — el `<label>` del truco de subida personalizada.
- [[06 Personalización de Controles (accent-color, appearance)]] — estilizar otros controles.
- [[index]] — los pseudoelementos en general.
