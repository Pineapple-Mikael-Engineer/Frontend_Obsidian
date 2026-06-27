---
title: IntersectionObserver — detectar visibilidad en el viewport
aliases:
  - IntersectionObserver
  - Intersection Observer
tags:
  - javascript
  - api/clase
  - browser
objeto: Web API
tipo: clase
retorna: IntersectionObserver
asincrono: true
draft: false
order: 1
---

# IntersectionObserver

> [!definicion]
> `IntersectionObserver` detecta cuándo un elemento entra o sale del viewport (o de otro elemento raíz) y entrega notificaciones asíncronas por lotes. Reemplaza el patrón `scroll` + `getBoundingClientRect()`, que fuerza reflows sincrónicos en el hilo principal. El callback recibe un array de `IntersectionObserverEntry` cada vez que la intersección de algún elemento observado cruza un umbral (`threshold`).

```js
const observer = new IntersectionObserver((entries, observer) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      cargarImagen(entry.target);
      observer.unobserve(entry.target); // dejar de observar una vez cargado
    }
  });
}, {
  threshold: 0.1,              // disparar cuando el 10 % sea visible
  rootMargin: "0px 0px -100px 0px" // contraer el viewport 100px desde abajo
});

document.querySelectorAll("img[data-src]").forEach(img => observer.observe(img));
```

## Opciones del constructor

| Opción | Tipo | Descripción |
|---|---|---|
| `root` | `Element` o `null` | Elemento contenedor de referencia. `null` usa el viewport |
| `rootMargin` | `string` (CSS) | Márgenes aplicados al root antes de calcular la intersección (p. ej. `"0px 0px -100px 0px"`) |
| `threshold` | `number` o `number[]` | Fracción visible (0–1) en la que se dispara el callback. Se puede pasar un array para múltiples umbrales |

## Propiedades de `IntersectionObserverEntry`

| Propiedad | Tipo | Descripción |
|---|---|---|
| `isIntersecting` | `boolean` | `true` si el elemento está visible según el umbral actual |
| `intersectionRatio` | `number` (0–1) | Porcentaje de la superficie del elemento actualmente visible |
| `boundingClientRect` | `DOMRectReadOnly` | Posición y dimensiones del elemento en el viewport |
| `intersectionRect` | `DOMRectReadOnly` | Área real de intersección entre el elemento y el root |
| `rootBounds` | `DOMRectReadOnly` o `null` | Bounds del root (del viewport si `root` es `null`) |
| `target` | `Element` | El elemento que disparó la entrada |
| `time` | `number` | Timestamp de alta resolución del evento |

## Métodos del observer

```js
observer.observe(elemento);      // comenzar a observar un elemento
observer.unobserve(elemento);    // dejar de observar un elemento concreto
observer.disconnect();           // dejar de observar todos los elementos
observer.takeRecords();          // obtener y vaciar las entradas pendientes sin esperar al callback
```

## Casos de uso

**Lazy loading de imágenes** — observar imágenes con `data-src`; cargar `src` al entrar al viewport y `unobserve` tras la carga (patrón del snippet inicial).

**Infinite scroll** — observar un elemento centinela al final de la lista; al hacerse visible, cargar la siguiente página.

**Animaciones al entrar al viewport** — añadir una clase CSS cuando `isIntersecting` pasa a `true`, con posibilidad de no revertir (usando `unobserve`).

**Analytics de visibilidad** — medir el tiempo que un anuncio o elemento de contenido permanece visible, registrando timestamps desde `entry.time`.

## Cómo funciona por dentro

El browser calcula las intersecciones de forma asíncrona y — en implementaciones modernas — fuera del hilo principal, lo que evita reflows forzados. Los callbacks se entregan en un microtask especial antes del paint (en la fase `requestAnimationFrame` del event loop). Cuando un elemento cruza varios umbrales en un mismo frame, el callback recibe todas las entradas en un solo array.

El uso de `getBoundingClientRect()` dentro de un scroll listener obliga al browser a calcular el layout de forma síncrona cada vez (layout thrashing). `IntersectionObserver` elimina ese problema completamente.

> [!tip]
> Al pasar `threshold: [0, 0.25, 0.5, 0.75, 1]` el callback se dispara cinco veces por elemento (una por cada cuarto de visibilidad), lo que permite implementar barras de progreso de lectura o modelos de visibilidad más granulares.

> [!warning]
> El callback se invoca **también al empezar a observar** un elemento, para entregar el estado inicial de intersección. No asumir que la primera llamada implica que el elemento acaba de entrar al viewport — puede ya estar visible o completamente fuera.

## Notas relacionadas

- [[index|Observers]] — visión general de los tres observers y cuándo usar cada uno.
- [[03 Resize Observer]] — si además de la visibilidad se necesita conocer el tamaño del elemento.
- [[../../07 Programación Asíncrona/index|Programación Asíncrona]] — el callback se ejecuta de forma asíncrona, no síncrona respecto al cambio.
