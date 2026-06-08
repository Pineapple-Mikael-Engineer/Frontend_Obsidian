---
title: FormData — crear desde formulario HTML
aliases:
  - new FormData
  - FormData constructor
tags:
  - javascript
  - api/clase
  - red
objeto: FormData
tipo: clase
retorna: FormData
muta: false
asincrono: false
draft: false
---

# Crear FormData desde Formulario

> [!definicion]
> El constructor `FormData` admite tres formas: vacío (`new FormData()`), capturando un `<form>` del DOM (`new FormData(formElement)`), o capturando un form más el botón que lo envió (`new FormData(formElement, submitter)`). En la forma con elemento, el navegador incluye automáticamente todos los campos con atributo `name` que no estén `disabled`, incluyendo archivos seleccionados en `<input type="file">`.

```js
// 1. FormData vacío — rellenar manualmente
const fd = new FormData();
fd.append('usuario', 'ana');
fd.append('avatar', fileBlob, 'foto.jpg');

// 2. Capturar formulario completo
const form = document.querySelector('#registro');
const fd = new FormData(form);

// 3. Incluir el botón submitter (ES2022+)
form.addEventListener('submit', (e) => {
  e.preventDefault();
  const fd = new FormData(form, e.submitter); // incluye el value del botón si tiene name
});
```

## Qué campos se incluyen (y qué no)

| Condición del campo | ¿Se incluye? |
|---|---|
| Tiene atributo `name` | Sí |
| Sin atributo `name` | No |
| `disabled` | No |
| `<input type="checkbox">` no marcado | No |
| `<input type="file">` con archivo | Sí, como objeto `File` |
| `<input type="file">` sin archivo | Sí, como string vacío |
| `<select multiple>` | Sí, una entrada por opción seleccionada |
| Botón `submit` sin `name` | No |
| Botón `submit` con `name` (si es submitter) | Sí (solo con tercer argumento) |

## Iteradores

`FormData` es iterable. Los tres métodos devuelven iteradores sobre las entradas del objeto:

```js
const fd = new FormData(form);

for (const [key, value] of fd.entries()) {
  console.log(key, value);
  // 'nombre' 'Ana García'
  // 'email'  'ana@example.com'
  // 'avatar' File { name: 'foto.jpg', size: 42318, type: 'image/jpeg' }
}

// Solo claves
for (const key of fd.keys()) { /* ... */ }

// Solo valores
for (const value of fd.values()) { /* ... */ }
```

El spread `[...fd]` y `Array.from(fd)` también funcionan y producen un array de pares `[key, value]`.

## Receta: capturar y enriquecer antes de enviar

```js
const form = document.querySelector('#pedido');

form.addEventListener('submit', async (e) => {
  e.preventDefault();

  const fd = new FormData(form);
  fd.append('timestamp', new Date().toISOString()); // campo extra
  fd.append('version', '2');

  const res = await fetch('/api/pedidos', { method: 'POST', body: fd });
  const data = await res.json();
  console.log('Pedido creado:', data.id);
});
```

## Cómo funciona por dentro

Al invocar `new FormData(formElement)`, el navegador ejecuta el mismo algoritmo de serialización que usaría el submit nativo del formulario (`application/x-www-form-urlencoded` solo para texto; `multipart/form-data` cuando hay archivos o el `enctype` lo indica). El resultado es un objeto mutable que actúa como lista ordenada de pares clave/valor — la misma clave puede aparecer varias veces (por ejemplo, múltiples checkboxes con el mismo `name`).

> [!tip]
> Para formularios con `<input type="file">`, `new FormData(form)` es la forma más directa de capturar los archivos: no hace falta acceder a `input.files` manualmente. Los objetos `File` conservan nombre, tamaño y tipo MIME.

> [!warning]
> Los campos sin `name` no se incluyen aunque tengan `id` o `class`. Es el atributo `name` el que identifica el campo en el FormData — un `<input id="email">` sin `name="email"` queda fuera.

## Notas relacionadas

- [[02 Métodos (append, get, set)|Métodos (append, get, set)]] — manipular entradas después de crear el FormData
- [[03 Enviar con Fetch|Enviar con Fetch]] — pasar el FormData como body de una petición fetch
