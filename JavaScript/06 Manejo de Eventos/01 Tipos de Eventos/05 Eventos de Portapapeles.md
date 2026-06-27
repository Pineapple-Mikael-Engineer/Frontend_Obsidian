---
title: Eventos de Portapapeles
aliases:
  - ClipboardEvent
  - clipboard events
  - eventos de copiar pegar
tags:
  - javascript
  - api/evento
  - eventos
draft: false
order: 5
---

# Eventos de Portapapeles

> [!definicion]
> Los eventos de portapapeles son instancias de `ClipboardEvent` que el navegador dispara cuando el usuario realiza operaciones de copiar, cortar o pegar. La interfaz expone `clipboardData`, un objeto `DataTransfer` con los datos intercambiados. La API moderna `navigator.clipboard` ofrece una alternativa basada en Promises más segura y con mayor capacidad, pero requiere HTTPS y un permiso explícito del usuario.

## Tabla de eventos

| Evento | Cuándo se dispara | Burbujea |
|---|---|---|
| copy | El usuario copia (Ctrl+C / Cmd+C o menú Copiar) | Sí |
| cut | El usuario corta (Ctrl+X / Cmd+X o menú Cortar) | Sí |
| paste | El usuario pega (Ctrl+V / Cmd+V o menú Pegar) | Sí |

Los tres eventos burbujean hasta `document` y pueden escucharse con delegación. Se disparan en el elemento que tiene el foco en el momento de la operación, o en `document.body` si ningún elemento específico tiene el foco.

## ClipboardEvent.clipboardData

`e.clipboardData` es un objeto `DataTransfer` con los métodos:
- `getData(tipo)` — devuelve los datos del portapapeles del tipo MIME indicado.
- `setData(tipo, valor)` — establece los datos (solo funciona en `copy` y `cut`).
- `clearData(tipo)` — elimina datos de un tipo.
- `items` — colección `DataTransferItemList` con todos los items del portapapeles.
- `files` — `FileList` con los archivos en el portapapeles (relevante en `paste` de imágenes).

Los tipos MIME más usados son `"text/plain"` para texto plano y `"text/html"` para texto con formato HTML.

## Leer en paste

```javascript
document.addEventListener('paste', (e) => {
  e.preventDefault(); // evita el pegado nativo

  const textoPuro = e.clipboardData.getData('text/plain');
  const textoHtml = e.clipboardData.getData('text/html');

  console.log('Texto plano:', textoPuro);
  console.log('HTML:       ', textoHtml); // vacío si se copió solo texto

  // Insertar en un contenteditable
  const seleccion = window.getSelection();
  if (seleccion.rangeCount > 0) {
    seleccion.deleteFromDocument();
    seleccion.getRangeAt(0).insertNode(document.createTextNode(textoPuro));
  }
});
```

## Escribir en copy/cut

```javascript
document.addEventListener('copy', (e) => {
  e.preventDefault(); // evita el comportamiento nativo

  const seleccion = window.getSelection().toString();
  const textoModificado = seleccion.toUpperCase(); // transformar antes de copiar

  e.clipboardData.setData('text/plain', textoModificado);
  e.clipboardData.setData('text/html', `<strong>${textoModificado}</strong>`);
});
```

## API moderna: navigator.clipboard

La API de Clipboard basada en Promises es la forma recomendada para leer/escribir el portapapeles desde código programático (un botón "Copiar", por ejemplo). Requiere contexto seguro (HTTPS o localhost) y, para lectura, el permiso `"clipboard-read"` del usuario.

```javascript
// Escribir al portapapeles (requiere activación del usuario — click, etc.)
async function copiarTexto(texto) {
  try {
    await navigator.clipboard.writeText(texto);
    console.log('Copiado al portapapeles');
  } catch (err) {
    console.error('No se pudo copiar:', err);
  }
}

// Leer del portapapeles (requiere permiso explícito del usuario)
async function leerPortapapeles() {
  try {
    const texto = await navigator.clipboard.readText();
    console.log('Contenido:', texto);
    return texto;
  } catch (err) {
    console.error('Sin permiso para leer portapapeles:', err);
    return null;
  }
}

// Leer con la Permissions API
async function pedirPermisoPortapapeles() {
  const permiso = await navigator.permissions.query({ name: 'clipboard-read' });
  if (permiso.state === 'denied') {
    console.warn('El usuario bloqueó el acceso al portapapeles');
  }
}
```

Para imágenes y otros tipos de datos, `navigator.clipboard.write()` acepta un array de `ClipboardItem`:

```javascript
async function copiarImagen(blob) {
  const item = new ClipboardItem({ [blob.type]: blob });
  await navigator.clipboard.write([item]);
}
```

## Receta: formatear texto al pegar

Un caso común en editors rich-text: el usuario pega texto desde Word o una página web y se necesita solo el texto plano, sin el HTML con estilos.

```javascript
const editor = document.getElementById('editor');

editor.addEventListener('paste', (e) => {
  e.preventDefault();

  let texto = e.clipboardData.getData('text/plain');

  // Normalizar: colapsar espacios múltiples, normalizar saltos de línea
  texto = texto
    .replace(/\r\n/g, '\n')
    .replace(/[ \t]{2,}/g, ' ')
    .trim();

  // Insertar como texto plano en la posición actual del cursor
  document.execCommand('insertText', false, texto);
  // Alternativa moderna si execCommand está deprecado:
  // const range = window.getSelection().getRangeAt(0);
  // range.deleteContents();
  // range.insertNode(document.createTextNode(texto));
});
```

## Receta: botón "Copiar al portapapeles"

```javascript
function configurarBotonCopiar(boton, obtenerTexto) {
  boton.addEventListener('click', async () => {
    const texto = typeof obtenerTexto === 'function' ? obtenerTexto() : obtenerTexto;

    try {
      await navigator.clipboard.writeText(texto);
      // Feedback visual temporal
      const textoOriginal = boton.textContent;
      boton.textContent = '¡Copiado!';
      boton.disabled = true;
      setTimeout(() => {
        boton.textContent = textoOriginal;
        boton.disabled = false;
      }, 2000);
    } catch {
      // Fallback para HTTP o navegadores sin soporte
      const textarea = document.createElement('textarea');
      textarea.value = texto;
      textarea.style.position = 'fixed';
      textarea.style.opacity = '0';
      document.body.appendChild(textarea);
      textarea.select();
      document.execCommand('copy');
      document.body.removeChild(textarea);
    }
  });
}

// Uso
const boton = document.getElementById('btn-copiar');
const codigo = document.getElementById('bloque-codigo');
configurarBotonCopiar(boton, () => codigo.textContent);
```

## Cómo funciona por dentro

Los eventos `copy`, `cut` y `paste` siguen el modelo de seguridad del navegador: solo se pueden activar programáticamente (con `document.execCommand`) desde una acción del usuario. La API `navigator.clipboard.writeText()` también requiere que la llamada ocurra en el contexto de un evento de usuario reciente (el navegador tiene una ventana de ~1 segundo). `navigator.clipboard.readText()` requiere además el permiso explícito `clipboard-read`, que el navegador solicita con un diálogo nativo. En el handler de `copy`/`paste`, `e.clipboardData` es el mecanismo de bajo nivel que el navegador expone para interceptar la operación; fuera de ese handler, no existe.

> [!tip]
> `navigator.clipboard` es la API correcta para botones de "copiar código" o "copiar enlace". Los eventos `copy`/`cut`/`paste` son para interceptar y transformar las operaciones nativas del usuario (por ejemplo, en un editor de texto online).

> [!warning]
> `document.execCommand('copy')` está deprecado pero aún ampliamente soportado. Usar `navigator.clipboard.writeText()` como primera opción y `execCommand` como fallback para compatibilidad con HTTP o contextos sin permiso.

## Notas relacionadas

- [[index]]
- [[06 Drag and Drop]]
- [[03 Eventos de Formulario]]
