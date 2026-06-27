---
title: Frontend — Documentación de referencia
aliases:
  - portada
  - inicio
  - home
tags:
  - html
  - css
  - javascript
draft: false
order: 1
---

# Frontend

> [!definicion]
> **Frontend** es el conjunto de tecnologías que el navegador ejecuta para construir lo que el usuario ve e interactúa. El navegador no es solo un visor de documentos: es un entorno de ejecución completo con motor de layout (HTML + CSS → píxeles), motor JavaScript, networking (Fetch, WebSockets), almacenamiento (localStorage, IndexedDB, Cache API), gráficos (Canvas, WebGL), audio/video, notificaciones, geolocalización y workers concurrentes. Frontend es programar para esa plataforma.

Las tres tecnologías se reparten responsabilidades sin solapamiento: **HTML** aporta estructura y significado, **CSS** controla la presentación y el movimiento, **JavaScript** programa el comportamiento y accede a las APIs del navegador. La separación no es arbitraria — permite que cada capa evolucione de forma independiente y que el navegador optimice cada una por separado (el layout en CSS corre en el compositor gráfico; el código JS en el motor V8/SpiderMonkey; el HTML es parseado una sola vez en el Critical Rendering Path).

---

## HTML — estructura y semántica

HTML no da estilos ni lógica, pero es la base sobre la que las otras dos capas actúan. Cada elemento HTML aporta **semántica**: `<article>` le dice al navegador (y a los lectores de pantalla y los crawlers) que ese bloque es contenido autónomo; `<nav>` marca la zona de navegación; `<button>` registra un elemento interactivo con teclado y accesibilidad listos de fábrica. Elegir el elemento correcto no es estética — afecta al SEO, a la accesibilidad y a cómo CSS y JS seleccionan los elementos.

```html
<form id="registro" novalidate>
  <fieldset>
    <legend>Cuenta nueva</legend>

    <label for="email">Correo electrónico</label>
    <input type="email" id="email" name="email" required autocomplete="email" />

    <label for="pass">Contraseña</label>
    <input type="password" id="pass" name="pass" minlength="8" required />
  </fieldset>

  <button type="submit">Crear cuenta</button>
  <output id="estado" aria-live="polite"></output>
</form>
```

El formulario usa `<fieldset>` + `<legend>` para agrupar campos semánticamente, `<label for>` para asociar etiquetas a inputs (clickable + lectores de pantalla), atributos de validación HTML nativos (`required`, `minlength`, `type="email"`) y `<output aria-live>` para anunciar respuestas del servidor sin recargar la página.

---

## CSS — presentación, layout y animación

CSS resuelve tres problemas distintos: **cómo organizar los elementos en pantalla** (Flexbox, Grid, posicionamiento), **cómo se ven** (colores, tipografía, sombras, bordes) y **cómo se mueven** (transiciones, `@keyframes`, animaciones del scroll). Las custom properties actúan como design tokens — definen el sistema de diseño en un lugar y se propagan a toda la hoja de estilos.

```css
/* Design tokens — un solo lugar para cambiar el tema completo */
:root {
  --color-acento:   #0070f3;
  --color-fondo:    #f9fafb;
  --espacio:        1rem;
  --radio:          8px;
  --transicion:     200ms ease;
}

/* Layout de aplicación con Grid: sidebar fija + contenido + panel lateral */
.app {
  display: grid;
  grid-template-columns: 240px 1fr 320px;
  grid-template-rows: 56px 1fr;
  grid-template-areas:
    "nav  header  header"
    "nav  main    aside";
  min-height: 100vh;
}

/* Tarjeta con entrada animada — sin una línea de JS */
.tarjeta {
  border-radius: var(--radio);
  padding: var(--espacio);
  opacity: 0;
  translate: 0 16px;
  transition: opacity var(--transicion), translate var(--transicion);
}
.tarjeta.visible {
  opacity: 1;
  translate: 0 0;
}

/* Estado hover enriquecido */
.btn-accion {
  background: var(--color-acento);
  color: #fff;
  border: none;
  border-radius: var(--radio);
  padding: .5rem 1.25rem;
  cursor: pointer;
  transition: filter var(--transicion), box-shadow var(--transicion);
}
.btn-accion:hover  { filter: brightness(1.1); box-shadow: 0 4px 12px #0070f340; }
.btn-accion:active { filter: brightness(.95); }
```

Grid define el esqueleto de la interfaz en CSS puro. Las custom properties permiten cambiar el tema completo modificando `:root`. La tarjeta anima su entrada solo con CSS: JS añade la clase `visible`, CSS ejecuta la transición en el hilo del compositor (sin bloquear el hilo principal).

---

## JavaScript — comportamiento, DOM y APIs del navegador

JS es la única de las tres tecnologías con acceso completo a las APIs del navegador. Además del DOM y los eventos, controla redes (Fetch, WebSockets), almacenamiento persistente (localStorage, IndexedDB), canvas 2D/WebGL, notificaciones, geolocalización, portapapeles, cámara/micrófono, y workers para cómputo paralelo. El patrón central es: el motor JS es single-threaded, pero las operaciones lentas (red, disco, timers) se delegan al entorno y regresan al hilo principal por el Event Loop.

```js
// Intersection Observer: activa la animación CSS al entrar al viewport
const observer = new IntersectionObserver(
  (entries) => entries.forEach(e => e.target.classList.toggle('visible', e.isIntersecting)),
  { threshold: 0.15 }
);
document.querySelectorAll('.tarjeta').forEach(el => observer.observe(el));

// Fetch con AbortController: cancela si el usuario navega antes de que responda
async function cargarUsuarios(signal) {
  const res = await fetch('/api/usuarios', { signal });
  if (!res.ok) throw new Error(`Error ${res.status}: ${res.statusText}`);
  return res.json();
}

const controller = new AbortController();
window.addEventListener('beforeunload', () => controller.abort());

try {
  const usuarios = await cargarUsuarios(controller.signal);
  renderizarTabla(usuarios);
} catch (err) {
  if (err.name !== 'AbortError') document.getElementById('estado').textContent = err.message;
}

// localStorage para persistir preferencias del usuario entre sesiones
function guardarTema(tema) {
  localStorage.setItem('tema', tema);
  document.documentElement.dataset.tema = tema;
}
const temaGuardado = localStorage.getItem('tema') ?? 'claro';
guardarTema(temaGuardado);
```

El `IntersectionObserver` detecta cuándo una tarjeta entra al viewport y añade la clase `visible` — el CSS hace el resto. El Fetch usa `AbortController` para cancelar la petición si el usuario cierra la pestaña. El `localStorage` persiste la preferencia de tema entre sesiones y la aplica antes del primer render para evitar el "flash" de tema incorrecto.

---

## Secciones de este vault

| Curso | Secciones | Qué cubre |
|---|---|---|
| HTML | 11 | Estructura semántica, formularios, multimedia, metadatos, SEO y accesibilidad |
| CSS | 10 | Modelo de caja, Flexbox, Grid, animaciones, responsive design, arquitectura y variables |
| JavaScript | 13 | Lenguaje base, DOM, eventos, asincronía, Fetch, almacenamiento, módulos, patrones y APIs del navegador |

## Cómo usar este vault

Cada sección tiene un **índice propio** que explica el alcance, narra la progresión interna y enlaza las subsecciones. Las subsecciones están numeradas — el número indica el orden pedagógico, no importancia relativa.

**Referencia puntual** (un método, una propiedad, un selector): usa la búsqueda global. **Aprendizaje en profundidad**: entra por el índice de la sección y sigue las notas en orden. **Explorar conexiones**: el grafo y los backlinks de cada nota muestran qué otros conceptos la referencian o dependen de ella.

## Notas relacionadas

- [[HTML/index|HTML — estructura y semántica]]
- [[CSS/index|CSS — presentación y layout]]
- [[JavaScript/index|JavaScript — comportamiento y APIs del navegador]]
