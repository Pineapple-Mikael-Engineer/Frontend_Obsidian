---
title: input type="file" — Subida de archivos
aliases:
  - input file
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: ninguno
vacio: true
draft: false
order: 11
---

# input file

> [!definicion]
> `<input type="file">` permite al usuario **seleccionar archivos** de su dispositivo para subirlos. Muestra un botón "Examinar" y el nombre del archivo elegido. Exige una configuración concreta en el `<form>` para que el contenido llegue al servidor.

```html
<form action="/subir" method="post" enctype="multipart/form-data">
  <label for="foto">Foto de perfil</label>
  <input type="file" id="foto" name="foto" accept="image/*" />
  <button type="submit">Subir</button>
</form>
```

## El formulario debe configurarse bien

> [!warning] Sin multipart, no llega el archivo
> Para subir archivos, el `<form>` **debe** llevar:
> - `method="post"` (nunca GET).
> - `enctype="multipart/form-data"`.
>
> Si falta el `enctype`, el formulario solo envía el **nombre** del archivo, no su contenido. Es el error nº 1 al implementar subidas. Detalle en [[01 Contenedor de Formulario (form) | `<form>`]].

## Atributos propios

| Atributo | Efecto |
|----------|--------|
| `accept` | Filtra los tipos ofrecidos (`image/*`, `.pdf`, `audio/*`) |
| `multiple` | Permite seleccionar **varios** archivos |
| `capture` | En móvil, abre directamente la cámara/micrófono (`user`/`environment`) |

```html
<!-- Varias imágenes desde la cámara trasera en móvil -->
<input type="file" accept="image/*" multiple capture="environment" />
```

## accept filtra, no valida

`accept` solo **sugiere** qué archivos mostrar en el diálogo; el usuario puede saltárselo ("todos los archivos"). La validación real de tipo y tamaño se hace en cliente con JavaScript (a través de la propiedad `files`) y, obligatoriamente, en el servidor.

## value es de solo lectura (seguridad)

> [!info] No se puede preseleccionar un archivo
> Por seguridad, el `value` de un `input file` **no se puede fijar** desde HTML ni JS: una página no puede elegir archivos del disco del usuario sin su intervención. Solo el usuario, a través del diálogo, establece el archivo. Por eso un campo file nunca aparece "prerrelleno".

## Procesar en cliente con JavaScript

La lista de archivos elegidos está en la propiedad `files` del input, lo que permite previsualizar o validar antes de enviar (detalle en el curso de JavaScript):

```js
input.addEventListener("change", () => {
  const archivo = input.files[0];
  console.log(archivo.name, archivo.size, archivo.type);
});
```

## Buenas prácticas

> [!tip] Recomendaciones
> - `method="post"` + `enctype="multipart/form-data"` siempre.
> - Usa `accept` para guiar la selección y `multiple`/`capture` según el caso.
> - Valida tipo y tamaño en cliente (UX) **y** en servidor (seguridad).
> - El control nativo es difícil de estilar: se suele ocultar y disparar desde un `<label>` o botón.

## Errores comunes

> [!warning] Trampas
> - **Olvidar `enctype="multipart/form-data"`**: no llega el archivo.
> - **Fiarse de `accept`** como validación: es solo un filtro del diálogo.
> - **Intentar prefijar `value`**: imposible por seguridad.

## Notas relacionadas

- [[01 Contenedor de Formulario (form)]] — `enctype` y `method` necesarios.
- [[01 Atributos Comunes de input]] — atributos compartidos.
- [[09 Validación de Formularios/index]] — validar tamaño/tipo en servidor.
