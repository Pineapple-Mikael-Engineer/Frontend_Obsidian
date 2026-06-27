---
title: Eventos de Ratón
aliases:
  - MouseEvent
  - eventos del mouse
tags:
  - javascript
  - api/evento
  - eventos
draft: false
order: 1
---

# Eventos de Ratón

> [!definicion]
> Los eventos de ratón son instancias de `MouseEvent` que el navegador dispara cuando el usuario interactúa con un elemento mediante el puntero. Cada evento lleva coordenadas en varios sistemas de referencia, información sobre qué botón se pulsó y el estado de las teclas modificadoras en el momento de la interacción.

## Tabla de eventos

| Evento | Cuándo se dispara | Burbujea |
|---|---|---|
| click | Botón izquierdo presionado y soltado sobre el mismo elemento | Sí |
| dblclick | Dos clicks rápidos sobre el mismo elemento | Sí |
| mousedown | Botón del ratón presionado (antes de soltar) | Sí |
| mouseup | Botón del ratón soltado | Sí |
| mousemove | El puntero se mueve sobre el elemento | Sí |
| mouseenter | El puntero entra en el elemento (no cuenta entrar en hijos) | No |
| mouseleave | El puntero sale del elemento (no cuenta salir hacia hijos) | No |
| mouseover | El puntero entra en el elemento o en cualquier hijo | Sí |
| mouseout | El puntero sale del elemento o de un hijo hacia otro nodo | Sí |
| contextmenu | Click derecho o tecla de menú contextual | Sí |

## Propiedades clave del MouseEvent

### Coordenadas

El objeto `MouseEvent` expone las coordenadas del puntero en cuatro sistemas de referencia distintos. Elegir el correcto evita cálculos de offset manuales.

```javascript
document.addEventListener('mousemove', (e) => {
  console.log('Viewport:   ', e.clientX, e.clientY);  // relativo a la ventana visible
  console.log('Documento:  ', e.pageX, e.pageY);       // relativo al documento completo (incluye scroll)
  console.log('Elemento:   ', e.offsetX, e.offsetY);   // relativo al elemento que recibió el evento
  console.log('Pantalla:   ', e.screenX, e.screenY);   // relativo a la pantalla física
});
```

- `clientX/Y` — la posición respecto al viewport. Es el valor más usado en aplicaciones de una página donde no hay scroll significativo.
- `pageX/Y` — igual que `clientX/Y` más el scroll acumulado del documento. Usar para posicionar elementos absolutos dentro del `document`.
- `offsetX/Y` — relativo al borde del elemento que disparó el evento (`e.target`). Cambia si el evento burbujea y `currentTarget` es un ancestro.
- `screenX/Y` — coordenadas físicas del monitor. Raramente necesario en web.

### Botón pulsado

```javascript
element.addEventListener('mousedown', (e) => {
  switch (e.button) {
    case 0: console.log('Izquierdo');  break;
    case 1: console.log('Rueda/Medio'); break;
    case 2: console.log('Derecho');    break;
    case 3: console.log('Atrás');      break;
    case 4: console.log('Adelante');   break;
  }

  // buttons es un bitmask para detectar múltiples botones simultáneos
  if (e.buttons & 1) console.log('Izquierdo mantenido');
  if (e.buttons & 2) console.log('Derecho mantenido');
  if (e.buttons & 4) console.log('Rueda mantenida');
});
```

`button` indica qué botón disparó el evento actual. `buttons` es un bitmask que indica qué botones están pulsados en ese momento, independientemente del botón que causó el evento. En `mousemove`, `button` siempre vale `0`; usar `buttons` para saber si se está arrastrando.

### Teclas modificadoras

```javascript
document.addEventListener('click', (e) => {
  if (e.ctrlKey || e.metaKey) {
    // Ctrl en Windows/Linux, Cmd en macOS
    console.log('Click + Control/Meta');
  }
  if (e.shiftKey) console.log('Click + Shift');
  if (e.altKey)   console.log('Click + Alt');
});
```

### relatedTarget

`e.relatedTarget` es el "otro" elemento implicado en un cambio de foco del puntero. En `mouseover` es el elemento que el puntero dejó; en `mouseout` es el elemento al que el puntero se dirige. Vale `null` si el puntero vino de fuera de la ventana o salió de ella.

## mouseenter/mouseleave vs mouseover/mouseout

Esta es la diferencia más importante al trabajar con hover en elementos anidados:

```html
<div id="padre">
  <span id="hijo">texto</span>
</div>
```

```javascript
const padre = document.getElementById('padre');

// mouseenter/mouseleave: solo se disparan al entrar/salir del padre,
// no cuando el puntero pasa entre padre e hijo.
padre.addEventListener('mouseenter', () => console.log('ENTER padre'));
padre.addEventListener('mouseleave', () => console.log('LEAVE padre'));

// mouseover/mouseout: se disparan también cuando el puntero entra en #hijo
// porque el evento burbujea desde hijo hasta padre.
padre.addEventListener('mouseover', () => console.log('OVER padre (quizás viene de hijo)'));
padre.addEventListener('mouseout',  () => console.log('OUT padre (quizás va a hijo)'));
```

**Regla práctica:** usar `mouseenter`/`mouseleave` para estilos de hover en un elemento concreto. Usar `mouseover`/`mouseout` cuando se necesite delegación de eventos o detectar movimiento entre hijos.

## Receta: drag manual

El drag nativo con `draggable` tiene limitaciones de estilo. Un drag manual con eventos de ratón da control total.

```javascript
function makeDraggable(el) {
  let offsetX = 0;
  let offsetY = 0;

  el.addEventListener('mousedown', (e) => {
    // Evitar selección de texto mientras se arrastra
    e.preventDefault();

    const rect = el.getBoundingClientRect();
    offsetX = e.clientX - rect.left;
    offsetY = e.clientY - rect.top;

    // Escuchar en document para no perder el puntero si se mueve rápido
    document.addEventListener('mousemove', onMouseMove);
    document.addEventListener('mouseup', onMouseUp);
  });

  function onMouseMove(e) {
    el.style.left = `${e.clientX - offsetX}px`;
    el.style.top  = `${e.clientY - offsetY}px`;
  }

  function onMouseUp() {
    document.removeEventListener('mousemove', onMouseMove);
    document.removeEventListener('mouseup', onMouseUp);
  }
}

// El elemento debe tener position: absolute o fixed
makeDraggable(document.getElementById('caja'));
```

El patrón clave: `mousedown` inicia; los listeners de `mousemove` y `mouseup` van en `document`, no en el elemento, para que el arrastre no se corte si el puntero sale del elemento rápidamente.

## Receta: menú contextual personalizado

```javascript
const menu = document.getElementById('menu-contextual');

document.addEventListener('contextmenu', (e) => {
  e.preventDefault(); // evita el menú nativo del navegador

  menu.style.left    = `${e.pageX}px`;
  menu.style.top     = `${e.pageY}px`;
  menu.style.display = 'block';
});

// Cerrar al hacer click en cualquier otro lado
document.addEventListener('click', () => {
  menu.style.display = 'none';
});
```

> [!tip]
> Para calcular la posición de tooltips o menús contextuales usa `e.pageX/Y`, no `e.clientX/Y`. Si el usuario ha hecho scroll, `clientX/Y` colocará el elemento relativo a la ventana y quedará desplazado respecto al cursor.

> [!warning]
> Registrar `mousemove` directamente en el `document` sin throttling puede saturar el hilo principal. Si el handler hace trabajo costoso (cálculos, repaints), aplicar throttle con `requestAnimationFrame` o con un flag de debounce para ejecutarlo como máximo una vez por frame.

## Notas relacionadas

- [[index]]
- [[06 Drag and Drop]]
- [[02 Eventos de Teclado]]
