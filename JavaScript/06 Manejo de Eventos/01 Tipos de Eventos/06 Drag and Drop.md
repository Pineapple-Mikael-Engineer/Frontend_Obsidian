---
title: Drag and Drop
aliases:
  - DragEvent
  - eventos de arrastre
  - drag and drop API
tags:
  - javascript
  - api/evento
  - eventos
draft: false
---

# Drag and Drop

> [!definicion]
> La API de Drag and Drop del navegador define una secuencia de eventos `DragEvent` que coordinan el arrastre de un elemento (la fuente) hacia una zona de destino (el drop target). `DragEvent` hereda de `MouseEvent` y añade la propiedad `dataTransfer`, un objeto `DataTransfer` que actúa como portapapeles temporal para transferir datos entre la fuente y el destino durante la operación.

## Tabla de eventos

| Evento | En qué elemento se dispara | Cuándo se dispara | Burbujea |
|---|---|---|---|
| dragstart | Elemento arrastrado | El usuario comienza a arrastrar | Sí |
| drag | Elemento arrastrado | Continuamente mientras se arrastra | Sí |
| dragend | Elemento arrastrado | El usuario suelta o cancela el arrastre | Sí |
| dragenter | Elemento destino | El puntero entra en la zona de drop | Sí |
| dragleave | Elemento destino | El puntero sale de la zona de drop | Sí |
| dragover | Elemento destino | Continuamente mientras el puntero está sobre la zona | Sí |
| drop | Elemento destino | El usuario suelta el elemento sobre la zona | Sí |

La secuencia completa de una operación de drag and drop es: `dragstart → drag (repetido) → dragenter → dragover (repetido) → drop → dragend`.

## Configurar el elemento arrastrable

Para que un elemento sea arrastrable, necesita el atributo `draggable="true"`. Los `<a>` con `href` y los `<img>` son arrastrables por defecto.

```html
<div id="tarjeta" draggable="true">Arrastra esto</div>
<div id="zona-drop">Suelta aquí</div>
```

## DataTransfer: pasar datos entre fuente y destino

`e.dataTransfer` es el mecanismo para enviar datos desde `dragstart` y recibirlos en `drop`. Acepta cualquier string con un tipo MIME como clave.

```javascript
const tarjeta = document.getElementById('tarjeta');

tarjeta.addEventListener('dragstart', (e) => {
  // Guardar los datos que se quieren transferir
  e.dataTransfer.setData('text/plain', tarjeta.id);
  e.dataTransfer.setData('application/json', JSON.stringify({ id: tarjeta.id, origen: 'columna-1' }));

  // Indicar el tipo de operación permitida
  e.dataTransfer.effectAllowed = 'move'; // 'copy', 'move', 'link', 'copyMove', 'all'

  tarjeta.classList.add('arrastrando');
});

tarjeta.addEventListener('dragend', () => {
  tarjeta.classList.remove('arrastrando');
});
```

## Configurar la zona de drop

La clave más importante: `dragover` debe llamar `e.preventDefault()`. Sin ello, el navegador no considera la zona como un drop target válido y el evento `drop` nunca se dispara.

```javascript
const zonaDrop = document.getElementById('zona-drop');

zonaDropzone.addEventListener('dragenter', (e) => {
  e.preventDefault();
  zonaDropzone.classList.add('drop-activo');
});

zonaDropzone.addEventListener('dragleave', (e) => {
  // Solo quitar el estilo si el puntero salió realmente del contenedor,
  // no si entró en un hijo (dragleave burbujea desde hijos)
  if (!zonaDropzone.contains(e.relatedTarget)) {
    zonaDropzone.classList.remove('drop-activo');
  }
});

zonaDropzone.addEventListener('dragover', (e) => {
  e.preventDefault(); // OBLIGATORIO para permitir drop
  e.dataTransfer.dropEffect = 'move'; // cambia el cursor: 'copy', 'move', 'link', 'none'
});

zonaDropzone.addEventListener('drop', (e) => {
  e.preventDefault(); // evita que el navegador abra el archivo si se arrastra uno
  zonaDropzone.classList.remove('drop-activo');

  const id = e.dataTransfer.getData('text/plain');
  const datos = JSON.parse(e.dataTransfer.getData('application/json') || '{}');

  const elemento = document.getElementById(id);
  if (elemento) zonaDropzone.appendChild(elemento);

  console.log('Recibido:', datos);
});
```

## effectAllowed y dropEffect

Estas dos propiedades controlan qué cursor muestra el navegador durante el arrastre:
- `dataTransfer.effectAllowed` se establece en `dragstart` en la fuente — indica qué operaciones están permitidas.
- `dataTransfer.dropEffect` se establece en `dragover`/`dragenter` en el destino — indica qué operación se realizará. Debe ser compatible con `effectAllowed`.

Si `dropEffect` no es compatible con `effectAllowed`, el cursor muestra "prohibido" y el `drop` no se dispara.

## Receta: lista reordenable por drag & drop

```javascript
function initListaOrdenable(lista) {
  let elementoArrastrado = null;

  lista.addEventListener('dragstart', (e) => {
    elementoArrastrado = e.target.closest('[draggable]');
    e.dataTransfer.effectAllowed = 'move';
    e.dataTransfer.setData('text/plain', ''); // Firefox lo requiere
    setTimeout(() => elementoArrastrado.classList.add('opaco'), 0);
  });

  lista.addEventListener('dragend', () => {
    elementoArrastrado?.classList.remove('opaco');
    elementoArrastrado = null;
    lista.querySelectorAll('.sobre-el').forEach(el => el.classList.remove('sobre-el'));
  });

  lista.addEventListener('dragover', (e) => {
    e.preventDefault();
    const destino = e.target.closest('[draggable]');
    if (!destino || destino === elementoArrastrado) return;

    destino.classList.add('sobre-el');
    const rect = destino.getBoundingClientRect();
    const mitad = rect.top + rect.height / 2;

    if (e.clientY < mitad) {
      lista.insertBefore(elementoArrastrado, destino);
    } else {
      lista.insertBefore(elementoArrastrado, destino.nextSibling);
    }
  });

  lista.addEventListener('dragleave', (e) => {
    e.target.closest('[draggable]')?.classList.remove('sobre-el');
  });

  lista.addEventListener('drop', (e) => {
    e.preventDefault();
  });
}
```

## Receta: zona de drop para subir archivos

`e.dataTransfer.files` es una `FileList` con los archivos del sistema operativo que el usuario soltó sobre la zona:

```javascript
const zonaArchivos = document.getElementById('zona-archivos');

zonaArchivos.addEventListener('dragover', (e) => {
  e.preventDefault();
  e.dataTransfer.dropEffect = 'copy';
  zonaArchivos.classList.add('drop-activo');
});

zonaArchivos.addEventListener('dragleave', () => {
  zonaArchivos.classList.remove('drop-activo');
});

zonaArchivos.addEventListener('drop', (e) => {
  e.preventDefault();
  zonaArchivos.classList.remove('drop-activo');

  const archivos = Array.from(e.dataTransfer.files);

  archivos.forEach(archivo => {
    console.log('Nombre:', archivo.name);
    console.log('Tipo:  ', archivo.type);
    console.log('Tamaño:', archivo.size, 'bytes');

    if (archivo.type.startsWith('image/')) {
      const reader = new FileReader();
      reader.onload = (ev) => {
        const img = document.createElement('img');
        img.src = ev.target.result;
        zonaArchivos.appendChild(img);
      };
      reader.readAsDataURL(archivo);
    }
  });
});
```

## Alternativa moderna

La API nativa de Drag and Drop tiene limitaciones para interfaces complejas: comportamiento inconsistente en móviles (touch no está soportado nativamente), dificultad para animar el elemento arrastrado, bugs históricos en Firefox con `dragleave`. Para interfaces de tablero (Kanban, editores), considerar la librería `@dnd-kit/core` (React) o implementar drag manual con Pointer Events, que unifica ratón y touch:

```javascript
// Pointer Events como alternativa portable
element.addEventListener('pointerdown', iniciarArrastre);
element.addEventListener('pointermove', moverElemento);
element.addEventListener('pointerup', finalizarArrastre);
// setPointerCapture para no perder el puntero al moverse rápido
element.setPointerCapture(e.pointerId);
```

## Cómo funciona por dentro

Durante un arrastre, el navegador toma una "snapshot" visual del elemento arrastrado (el ghost image). Se puede personalizar con `e.dataTransfer.setDragImage(imagen, offsetX, offsetY)` en `dragstart`. El objeto `DataTransfer` es solo accesible durante los eventos de drag — fuera de ellos, `getData()` devuelve cadena vacía y `setData()` no tiene efecto. Esto es una restricción de seguridad: los datos del portapapeles no deben ser legibles por código que corre fuera de una operación de usuario.

> [!tip]
> Firefox requiere que `setData()` sea llamado con al menos un valor en `dragstart` para iniciar la operación de drag. Una práctica común es `e.dataTransfer.setData('text/plain', '')` como llamada mínima obligatoria cuando los datos reales se pasan por otro mecanismo (como una variable de módulo).

> [!warning]
> `dragleave` se dispara cuando el puntero entra en un elemento hijo de la zona de drop, porque el hijo no es el zona en sí. Para detectar si el puntero realmente salió de la zona, verificar `!zonaDropzone.contains(e.relatedTarget)` en el handler de `dragleave`.

## Notas relacionadas

- [[index]]
- [[01 Eventos de Ratón]]
- [[05 Eventos de Portapapeles]]
