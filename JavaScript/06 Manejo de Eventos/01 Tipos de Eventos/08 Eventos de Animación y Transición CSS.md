---
title: Eventos de Animación y Transición CSS
aliases:
  - AnimationEvent
  - TransitionEvent
  - eventos de animación
  - eventos de transición
tags:
  - javascript
  - api/evento
  - eventos
draft: false
order: 8
---

# Eventos de Animación y Transición CSS

> [!definicion]
> Los eventos de animación (`AnimationEvent`) y de transición (`TransitionEvent`) permiten a JavaScript sincronizarse con el motor de renderizado del navegador: detectar cuándo una animación o transición CSS empieza, termina o se cancela. Son el mecanismo correcto para encadenar animaciones, limpiar clases CSS al terminar o manipular el DOM justo después de una animación de salida.

## Tabla de eventos

| Evento | Tipo | Cuándo se dispara | Burbujea |
|---|---|---|---|
| animationstart | AnimationEvent | La animación CSS comienza | Sí |
| animationend | AnimationEvent | La animación CSS termina | Sí |
| animationiteration | AnimationEvent | Una iteración de la animación completa (si hay más de una) | Sí |
| animationcancel | AnimationEvent | La animación se cancela antes de terminar | Sí |
| transitionstart | TransitionEvent | La transición CSS comienza | Sí |
| transitionend | TransitionEvent | La transición CSS termina | Sí |
| transitioncancel | TransitionEvent | La transición se cancela antes de terminar | Sí |
| transitionrun | TransitionEvent | La transición es creada (antes del delay) | Sí |

## Propiedades de AnimationEvent

```javascript
elemento.addEventListener('animationend', (e) => {
  console.log('animationName:', e.animationName); // el nombre en @keyframes
  console.log('elapsedTime:  ', e.elapsedTime);   // segundos transcurridos
  console.log('pseudoElement:', e.pseudoElement);  // '::before', '::after' o ''
});
```

- `animationName` — el nombre definido en `@keyframes nombre { ... }`. Permite distinguir qué animación terminó si un elemento tiene varias.
- `elapsedTime` — tiempo transcurrido en segundos desde que empezó la animación (excluye delays).
- `pseudoElement` — si la animación es en un `::before` o `::after`, devuelve `"::before"` o `"::after"`; si es en el elemento principal, devuelve `""`.

## Propiedades de TransitionEvent

```javascript
elemento.addEventListener('transitionend', (e) => {
  console.log('propertyName:', e.propertyName); // propiedad CSS que transitó
  console.log('elapsedTime: ', e.elapsedTime);  // duración de la transición en segundos
  console.log('pseudoElement:', e.pseudoElement);
});
```

- `propertyName` — la propiedad CSS que completó su transición: `"opacity"`, `"transform"`, `"background-color"`, etc. Si el elemento tiene `transition` en múltiples propiedades, `transitionend` se dispara una vez por cada propiedad.
- `elapsedTime` — duración de la transición (sin incluir el delay).

## Por qué transitionend se dispara múltiples veces

Si un elemento tiene `transition: opacity 0.3s, transform 0.5s`, el evento `transitionend` se dispara dos veces: una para `opacity` y otra para `transform`, en distintos momentos. Usar `e.propertyName` para escuchar solo la que importa.

```javascript
elemento.addEventListener('transitionend', (e) => {
  if (e.propertyName !== 'opacity') return; // ignorar las otras propiedades
  procesarFinDeTransicion();
});
```

## Receta: eliminar elemento del DOM al terminar animación de salida

El patrón estándar para animaciones de salida: aplicar clase CSS que dispara la animación, luego eliminar el elemento del DOM cuando termina.

```css
.elemento-saliendo {
  animation: deslizarAfuera 0.4s ease-in forwards;
}

@keyframes deslizarAfuera {
  from { opacity: 1; transform: translateX(0); }
  to   { opacity: 0; transform: translateX(100px); }
}
```

```javascript
function eliminarConAnimacion(elemento) {
  elemento.classList.add('elemento-saliendo');

  elemento.addEventListener('animationend', () => {
    elemento.remove();
  }, { once: true }); // { once: true } elimina el listener automáticamente
}

// Uso
document.querySelectorAll('.btn-eliminar').forEach(btn => {
  btn.addEventListener('click', (e) => {
    const tarjeta = e.target.closest('.tarjeta');
    eliminarConAnimacion(tarjeta);
  });
});
```

`{ once: true }` en el tercer argumento de `addEventListener` hace que el listener se elimine automáticamente después de ejecutarse una vez — evita la necesidad de llamar `removeEventListener` manualmente.

## Receta: encadenar transiciones

Ejecutar una segunda transición justo cuando termina la primera:

```javascript
const elemento = document.getElementById('caja');

// Fase 1: deslizar hacia arriba
elemento.style.transition = 'transform 0.4s ease-out';
elemento.style.transform = 'translateY(-20px)';

// Fase 2: cuando termina, hacer fade out
elemento.addEventListener('transitionend', (e) => {
  if (e.propertyName !== 'transform') return;

  elemento.style.transition = 'opacity 0.3s ease-in';
  elemento.style.opacity = '0';
}, { once: true });
```

Versión más limpia con clases CSS:

```javascript
function ejecutarSecuencia(elemento) {
  elemento.classList.add('fase-1');

  elemento.addEventListener('transitionend', (e) => {
    if (e.propertyName !== 'transform') return;
    elemento.classList.remove('fase-1');
    elemento.classList.add('fase-2');
  }, { once: true });
}
```

## Receta: promisificar transitionend

Para usar con `async/await` en secuencias de animación complejas:

```javascript
function esperarTransicion(elemento, propiedad = null) {
  return new Promise((resolve) => {
    function handler(e) {
      if (propiedad && e.propertyName !== propiedad) return;
      elemento.removeEventListener('transitionend', handler);
      resolve(e);
    }
    elemento.addEventListener('transitionend', handler);
  });
}

// Uso
async function animarEntrada(el) {
  el.classList.add('entrando');
  await esperarTransicion(el, 'opacity');
  el.classList.remove('entrando');
  el.classList.add('visible');
  console.log('Animación completada');
}
```

## Cuándo usar eventos de animación vs otros mecanismos

- **`animationend` / `transitionend`**: cuando el timing importa para la lógica de la aplicación (remover del DOM, encadenar animaciones, actualizar estado).
- **`requestAnimationFrame`**: cuando se necesita control frame a frame de una animación JavaScript, no CSS.
- **Web Animations API (`element.animate()`)**: alternativa moderna que retorna una `Animation` con Promises (`animation.finished`), evitando la necesidad de escuchar eventos DOM para detectar el final.

```javascript
// Web Animations API — alternativa moderna
const animation = elemento.animate(
  [{ opacity: 1, transform: 'translateX(0)' },
   { opacity: 0, transform: 'translateX(100px)' }],
  { duration: 400, easing: 'ease-in', fill: 'forwards' }
);

// La Promise se resuelve cuando la animación termina
animation.finished.then(() => elemento.remove());
```

## Cómo funciona por dentro

Los eventos de transición y animación los despacha el compositor del navegador (compositor thread), sincronizados con el ciclo de rendering. A diferencia de los eventos de usuario (click, keydown), que entran en la cola de tareas normales, estos eventos se despachan al final del frame de rendering en el que la animación cambió de estado. Esto garantiza que cuando el handler corre, el layout del frame ya está calculado y las propiedades computadas son las definitivas.

`animationcancel` y `transitioncancel` se disparan cuando la animación/transición se interrumpe antes de terminar: por ejemplo, si se cambia `display` a `none`, si se elimina el elemento, o si otra regla CSS contradice la animación. Son útiles para limpiar estado incluso en caminos de error.

> [!tip]
> Para animaciones de entrada/salida en listas dinámicas, la librería FLIP (First Last Invert Play) usa `getBoundingClientRect()` antes y después de un cambio en el DOM para calcular la diferencia y aplicar una transición CSS suave. Es más performante que animar `top`/`left` directamente porque usa `transform`, que no causa reflow.

> [!warning]
> `transitionend` NO se dispara si la transición no llegó a ocurrir: si la propiedad ya estaba en el valor destino, si `display: none` cancela la transición, o si la propiedad no es animable. Si la lógica depende de que `transitionend` siempre se dispare, añadir un timeout de seguridad como fallback: `const fallback = setTimeout(() => limpiar(), duracion + 50)` y cancelarlo en `transitionend`.

## Notas relacionadas

- [[index]]
- [[07 Eventos de Medios]]
- [[01 Eventos de Ratón]]
