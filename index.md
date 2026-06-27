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
> **Frontend** es la capa del desarrollo web que el usuario ve y con la que interactúa directamente. Se compone de tres tecnologías complementarias que el navegador combina en el Critical Rendering Path: **HTML** define la estructura semántica del contenido, **CSS** controla su presentación visual, y **JavaScript** añade comportamiento dinámico. Sin HTML no hay nada que mostrar; sin CSS, solo texto plano sin forma; sin JS, una página estática sin respuesta.

La relación entre las tres es de capas superpuestas, cada una con una responsabilidad exclusiva. HTML es el único que el navegador necesita para mostrar algo — las otras dos son progresivas. Esta separación de responsabilidades no es solo buena práctica: está en la base de cómo funciona la plataforma web.

```html
<!-- HTML: define estructura y significado -->
<button class="btn" id="guardar">Guardar cambios</button>
```

```css
/* CSS: controla la apariencia */
.btn { background: #0070f3; color: #fff; border: none; border-radius: 6px; padding: .5rem 1.25rem; cursor: pointer; }
.btn:hover { background: #005fd3; }
```

```js
// JavaScript: añade comportamiento
document.getElementById('guardar').addEventListener('click', async () => {
  const res = await fetch('/api/guardar', { method: 'POST', body: obtenerDatos() });
  if (res.ok) mostrarConfirmacion();
});
```

## Secciones

| Curso | Secciones | Qué cubre |
|---|---|---|
| HTML | 11 | Estructura semántica, formularios, multimedia, metadatos, SEO y accesibilidad |
| CSS | 10 | Modelo de caja, layout moderno (Flexbox/Grid), animaciones, responsive design y arquitectura |
| JavaScript | 13 | Lenguaje base, DOM, eventos, asincronía, APIs del navegador, módulos y patrones |

## Cómo usar este vault

Cada sección tiene un **índice propio** que explica su alcance, muestra la progresión interna y apunta a las subsecciones. Las subsecciones están numeradas — el número indica el orden pedagógico recomendado, no jerarquía de importancia.

**Para referencia puntual** (un método, una propiedad, un concepto): la búsqueda global localiza cualquier nota por nombre o contenido. **Para entender un tema en profundidad**: entra por el índice de la sección y sigue las notas en orden. **Para explorar conexiones**: el grafo y los backlinks de cada nota muestran qué otros conceptos la referencian.

## Notas relacionadas

- [[HTML/index|HTML — estructura y semántica]]
- [[CSS/index|CSS — presentación y layout]]
- [[JavaScript/index|JavaScript — comportamiento e interactividad]]
