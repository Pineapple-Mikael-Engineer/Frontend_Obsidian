---
title: Eventos de Formulario
aliases:
  - form events
  - eventos de formulario
tags:
  - javascript
  - api/evento
  - eventos
draft: false
---

# Eventos de Formulario

> [!definicion]
> Los eventos de formulario notifican cambios en el estado de los controles de entrada (`<input>`, `<select>`, `<textarea>`) y del `<form>` como unidad. Son la base de cualquier sistema de validación, búsqueda en vivo o gestión de estado de formularios en JavaScript. Las interfaces implicadas son `Event` (para `submit`, `reset`, `change`, `input`, `invalid`) y `FocusEvent` (para `focus`, `blur`, `focusin`, `focusout`).

## Tabla de eventos

| Evento | Cuándo se dispara | Burbujea |
|---|---|---|
| submit | El formulario se envía (botón submit, Enter en input) | Sí |
| reset | El formulario se resetea (botón reset o `form.reset()`) | Sí |
| change | El valor cambia y el control pierde el foco (texto), o al seleccionar (checkbox, radio, select) | Sí |
| input | El valor cambia en tiempo real (cada pulsación, pegado, borrado) | Sí |
| focus | El control gana el foco | No |
| blur | El control pierde el foco | No |
| focusin | El control gana el foco | Sí |
| focusout | El control pierde el foco | Sí |
| invalid | La validación nativa HTML5 falla al intentar enviar | Sí |

## submit

`submit` se dispara en el elemento `<form>`, no en el botón que lo lanza. Esto es importante: poner el listener en `<button type="submit">` no funciona para capturar el envío.

```javascript
const form = document.getElementById('mi-form');

form.addEventListener('submit', (e) => {
  e.preventDefault(); // detener el envío HTTP nativo

  const datos = new FormData(e.target);
  // Validación manual
  if (!datos.get('email')) {
    mostrarError('El email es obligatorio');
    return;
  }
  enviarAlServidor(Object.fromEntries(datos));
});
```

`e.target` dentro del handler de `submit` siempre es el `<form>`, independientemente de qué botón disparó el envío. `new FormData(form)` serializa todos los campos con `name` en un objeto iterable.

## change vs input

Esta distinción es crítica para evitar handlers que se disparan demasiado (o muy poco):

```javascript
const input = document.getElementById('campo-texto');

// change: solo cuando pierde foco con un valor diferente al que tenía al ganar foco
input.addEventListener('change', (e) => {
  console.log('Valor final al salir del campo:', e.target.value);
});

// input: cada vez que el valor cambia, incluso con el campo activo
input.addEventListener('input', (e) => {
  console.log('Valor actual:', e.target.value); // se llama en cada keystroke
});
```

Para **checkbox** y **radio**, `change` se dispara inmediatamente al marcar/desmarcar, sin esperar a perder el foco. Para **select**, `change` se dispara al elegir una opción diferente. En estos controles, `input` y `change` son equivalentes.

## focus / blur y focusin / focusout

`focus` y `blur` no burbujean, lo que impide usar delegación de eventos directamente. Sus contrapartes `focusin` y `focusout` son idénticos en semántica pero sí burbujean, permitiendo un único listener en el form o en un contenedor padre.

```javascript
// Con focus/blur: necesita listener en cada input individualmente
document.querySelectorAll('input').forEach(input => {
  input.addEventListener('focus', () => input.parentElement.classList.add('activo'));
  input.addEventListener('blur',  () => input.parentElement.classList.remove('activo'));
});

// Con focusin/focusout: un solo listener en el form usando delegación
form.addEventListener('focusin', (e) => {
  if (e.target.matches('input, textarea, select')) {
    e.target.parentElement.classList.add('activo');
  }
});
form.addEventListener('focusout', (e) => {
  if (e.target.matches('input, textarea, select')) {
    e.target.parentElement.classList.remove('activo');
  }
});
```

`FocusEvent` expone `e.relatedTarget`: en `focus`/`focusin` es el elemento que perdió el foco; en `blur`/`focusout` es el elemento que va a ganar el foco. Puede ser `null` si el foco vino de fuera de la página o si el navegador no soporta la propiedad.

## invalid

El evento `invalid` se dispara en cada control que falla la validación nativa HTML5 cuando el usuario intenta enviar el formulario. Se puede usar para personalizar mensajes de error sin suprimir la validación del navegador.

```javascript
form.querySelectorAll('input').forEach(input => {
  input.addEventListener('invalid', (e) => {
    e.preventDefault(); // evita el tooltip nativo del navegador
    mostrarErrorPersonalizado(e.target, e.target.validationMessage);
  });
});
```

`e.target.validationMessage` contiene el mensaje de error generado por el navegador según la restricción que falló (`required`, `minlength`, `pattern`, `type="email"`, etc.). `e.target.validity` es un `ValidityState` con flags booleanos (`valueMissing`, `typeMismatch`, `patternMismatch`, etc.).

## Cómo funciona por dentro

El evento `submit` se dispara cuando el usuario activa el botón submit o presiona Enter en un input de texto (en forms de un solo campo o con `type="submit"`). El navegador construye la URL o el body de la petición HTTP antes de despachar el evento, por eso `e.preventDefault()` aún puede cancelar la petición.

La diferencia entre `change` e `input` está en el modelo de "committed value" del DOM: el navegador considera que el valor de un `<input type="text">` solo está "comprometido" cuando el usuario abandona el campo. `input`, en cambio, sigue el valor del "working value" mientras el usuario escribe.

## Receta: validación en submit con e.preventDefault()

```javascript
form.addEventListener('submit', (e) => {
  e.preventDefault();
  limpiarErrores();

  const nombre  = form.nombre.value.trim();
  const email   = form.email.value.trim();
  const errores = [];

  if (!nombre) errores.push({ campo: 'nombre', msg: 'El nombre es obligatorio' });
  if (!email || !email.includes('@')) errores.push({ campo: 'email', msg: 'Email inválido' });

  if (errores.length > 0) {
    errores.forEach(({ campo, msg }) => mostrarError(campo, msg));
    form[errores[0].campo].focus();
    return;
  }

  enviarFormulario({ nombre, email });
});
```

## Receta: búsqueda en tiempo real con input y debounce

Sin debounce, `input` puede disparar una petición HTTP por cada letra que el usuario escribe. Con debounce, la petición se retrasa hasta que el usuario deja de escribir por un tiempo.

```javascript
function debounce(fn, retraso) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), retraso);
  };
}

const buscar = debounce((query) => {
  if (query.length < 2) return;
  fetch(`/api/buscar?q=${encodeURIComponent(query)}`)
    .then(r => r.json())
    .then(resultados => renderizarResultados(resultados));
}, 300);

document.getElementById('campo-busqueda').addEventListener('input', (e) => {
  buscar(e.target.value.trim());
});
```

> [!tip]
> Cuando necesites leer el valor actual de un input dentro de un handler de `submit`, usar `e.target.elements['nombre-campo'].value` o `new FormData(e.target)` es más robusto que tener referencias a los inputs capturadas al cargar la página, porque el form puede haber sido modificado dinámicamente.

> [!warning]
> `focus` y `blur` no burbujean. Un patrón común de bug es poner el listener de `blur` en el contenedor esperando que funcione con delegación — no funcionará. Usar `focusout` para delegación o poner el listener directamente en el control.

## Notas relacionadas

- [[index]]
- [[02 Eventos de Teclado]]
- [[04 Eventos de Ventana y Documento]]
