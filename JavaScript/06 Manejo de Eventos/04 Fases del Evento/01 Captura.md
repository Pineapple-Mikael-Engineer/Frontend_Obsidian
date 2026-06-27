---
title: Fase de Captura — El evento desciende hasta el target
aliases:
  - captura de eventos
  - event capture
  - capture phase
tags:
  - javascript
  - api/concepto
  - eventos
draft: false
order: 1
---

# Captura

> [!definicion]
> La fase de captura es la primera de las tres fases de propagación. El evento parte desde `window` y desciende por cada ancestro del árbol DOM hasta llegar al `target`. Los listeners registrados con `{ capture: true }` (o el booleano `true` como tercer argumento) se ejecutan durante este descenso, antes de que el evento alcance el elemento que lo originó.

```js
document.addEventListener('click', (e) => {
  console.log('captura en document — antes de llegar al target');
  console.log('target será:', e.target);
}, true);  // tercer argumento true = fase de captura

// Equivalente con objeto de opciones:
document.addEventListener('click', (e) => {
  console.log('captura');
}, { capture: true });
```

## Cómo funciona

Al producirse un click en `<li>`:

```
window          ← captura (si hay listener aquí con capture:true)
  document      ← captura
    <html>      ← captura
      <body>    ← captura
        <ul>    ← captura (se ejecutan listeners con capture:true)
          <li>  ← target (fase 2, no es captura ya)
```

El evento nunca "sube" en la fase de captura — el orden es estrictamente top-down.

## Casos de uso

**Logging global antes de que el evento sea procesado:**

```js
window.addEventListener('click', (e) => {
  analytics.track('click', { element: e.target.tagName, time: e.timeStamp });
}, true);  // captura todos los clics antes que cualquier handler de burbujeo
```

**Interceptar y cancelar eventos antes de que lleguen al target:**

```js
formProtegido.addEventListener('submit', (e) => {
  if (!sesionActiva()) {
    e.preventDefault();
    e.stopPropagation();
    redirigirALogin();
  }
}, true);
```

**Foco global: detectar cambios de foco antes de que los componentes reaccionen.** Los eventos `focus`/`blur` no burbujean, pero sí se propagan en captura, lo que permite escucharlos globalmente desde un ancestro:

```js
document.addEventListener('focus', (e) => {
  console.log('foco en:', e.target);
}, true);  // sin true, document nunca recibe focus de elementos hijos
```

> [!info]
> La captura es rara en aplicaciones cotidianas. La mayor parte del código de UI usa el burbujeo (default). La captura es útil principalmente para instrumentación (logging, analíticas), interceptación de accesibilidad, y casos en que se necesita actuar antes de que los handlers del target procesen el evento.

> [!warning]
> Un listener de captura que llame `e.stopPropagation()` impide que el evento alcance su `target`. Esto puede romper componentes que esperan recibir el evento en la fase target o de burbujeo. Usar con precaución en interceptores globales.

## Notas relacionadas

- [[02 Target|Fase Target]]
- [[03 Burbujeo|Burbujeo]]
- [[04 Fases del Evento/index|Fases del Evento]]
- [[02 Opciones (once, passive, capture)|Opciones de addEventListener]]
