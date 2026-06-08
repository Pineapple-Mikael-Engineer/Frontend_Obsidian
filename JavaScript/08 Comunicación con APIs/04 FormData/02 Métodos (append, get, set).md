---
title: FormData — métodos append, get, set y resto de la API
aliases:
  - FormData.append
  - FormData.set
  - FormData.get
  - FormData métodos
tags:
  - javascript
  - api/clase
  - red
objeto: FormData
tipo: clase
muta: true
asincrono: false
draft: false
---

# Métodos de FormData (append, get, set…)

> [!definicion]
> `FormData` expone una API de instancia para leer y modificar sus entradas. La distinción clave entre `append` y `set`: `append` añade una entrada nueva sin eliminar las existentes bajo la misma clave (permite valores múltiples), mientras que `set` reemplaza todas las entradas de esa clave por una sola. El valor puede ser `string`, `Blob` o `File`; para archivos se acepta un tercer argumento con el nombre del fichero.

```js
const fd = new FormData();

fd.append('tags', 'js');
fd.append('tags', 'web');      // dos entradas con clave 'tags'
fd.getAll('tags');             // → ['js', 'web']

fd.set('tags', 'frontend');   // elimina ambas, queda una
fd.getAll('tags');             // → ['frontend']
```

## Tabla de métodos

| Método | Descripción | Notas |
|---|---|---|
| `append(nombre, valor)` | Añade un par nuevo; no elimina duplicados | Permite múltiples valores por clave |
| `append(nombre, blob, nombreArchivo)` | Añade un Blob/File con nombre de fichero | El tercer arg es obligatorio para Blob crudo |
| `set(nombre, valor)` | Sustituye todos los valores de esa clave | Si no existe, la crea |
| `set(nombre, blob, nombreArchivo)` | Igual que append pero reemplazando | — |
| `get(nombre)` | Primer valor de la clave, o `null` | Retorna `string` o `File` |
| `getAll(nombre)` | Array con todos los valores de la clave | Array vacío si no existe |
| `has(nombre)` | `true` si la clave existe | — |
| `delete(nombre)` | Elimina todas las entradas de esa clave | No lanza si no existe |
| `forEach(cb)` | Itera `(value, key, fd)` por cada entrada | Incluye duplicados |

## Trabajar con archivos

Para añadir un archivo desde un `<input type="file">` o desde un `Blob` construido en código:

```js
// Desde un input file
const input = document.querySelector('#avatar');
const archivo = input.files[0];         // objeto File
fd.append('avatar', archivo);           // el nombre del fichero se toma del File

// Desde un Blob crudo (sin nombre nativo)
const blob = new Blob(['contenido'], { type: 'text/plain' });
fd.append('documento', blob, 'informe.txt'); // tercer argumento necesario

// Múltiples archivos
const inputMultiple = document.querySelector('#galeria');
for (const file of inputMultiple.files) {
  fd.append('fotos', file);             // cada archivo bajo la misma clave
}
fd.getAll('fotos');                     // → [File, File, File, …]
```

## Receta: construir un FormData con validación previa

```js
async function subirPerfil(nombre, email, avatarInput) {
  const archivo = avatarInput.files[0];

  if (!archivo) throw new Error('Se requiere una imagen de avatar');
  if (archivo.size > 2 * 1024 * 1024) throw new Error('Imagen mayor a 2 MB');

  const fd = new FormData();
  fd.set('nombre', nombre.trim());
  fd.set('email', email.toLowerCase());
  fd.set('avatar', archivo);            // nombre del fichero viene del File

  const res = await fetch('/api/perfil', { method: 'PUT', body: fd });
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json();
}
```

## Cómo funciona por dentro

Internamente, `FormData` mantiene una lista ordenada de tuplas `(nombre, valor)`. A diferencia de un objeto plano (`{}`), admite claves duplicadas — el modelo es el mismo que el de los campos de un formulario HTML donde varios checkboxes pueden tener el mismo `name`. `get` y `getAll` leen esa lista; `set` la filtra y reemplaza; `append` la extiende. Cuando el objeto se pasa como `body` a `fetch`, el motor lo serializa linealmente recorriendo esa lista de arriba a abajo.

> [!tip]
> Usar `set` en lugar de `append` cuando el FormData se construye programáticamente y se quiere garantizar un solo valor por clave. Usar `append` cuando se replican semanticas de checkboxes o inputs de archivo múltiple.

> [!warning]
> `fd.get('clave')` retorna `null` (no `undefined`) si la clave no existe. Comparar siempre con `!== null` o usar `fd.has('clave')` antes. Además, el valor de un campo de texto siempre llega como `string`, aunque el input fuera `type="number"` — la conversión es responsabilidad del código receptor.

## Notas relacionadas

- [[01 Crear desde Formulario|Crear desde Formulario]] — cómo construir el FormData desde un `<form>`
- [[03 Enviar con Fetch|Enviar con Fetch]] — enviar el FormData con fetch
