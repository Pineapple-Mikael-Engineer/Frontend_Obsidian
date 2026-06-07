---
title: Node.contains — Comprobar si un nodo es descendiente
aliases:
  - contains
  - Node.contains
tags:
  - javascript
  - api/metodo
  - dom
objeto: Node
tipo: metodo
retorna: boolean
muta: false
asincrono: false
draft: false
---

# `Node.contains`

> [!definicion]
> `nodo.contains(otroNodo)` devuelve `true` si `otroNodo` es el propio nodo o cualquiera de sus descendientes (a cualquier profundidad). Devuelve `false` en caso contrario, incluyendo cuando `otroNodo` es `null`. Equivale a preguntar "¿está `otroNodo` dentro de `nodo` o es el mismo nodo?".

```js
const modal = document.querySelector('.modal');
const btnCerrar = modal.querySelector('.btn-cerrar');

modal.contains(btnCerrar);   // → true  (descendiente)
modal.contains(modal);       // → true  (el propio nodo)
modal.contains(document.body); // → false (body no está dentro del modal)
modal.contains(null);        // → false
```

## Firma

```text
Node.contains(otroNodo: Node | null): boolean
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `otroNodo` | `Node` o `null` | El nodo a comprobar |

**Retorna:** `true` si `otroNodo` es el propio nodo o un descendiente; `false` en cualquier otro caso.

## Caso de uso principal: detectar click fuera de un elemento

El patrón más frecuente de `contains` es cerrar un menú, modal o dropdown cuando el usuario hace click fuera de él. La clave es que `e.target` puede ser cualquier elemento de la página, incluyendo los hijos del componente:

```js
const menu = document.querySelector('.menu-desplegable');

document.addEventListener('click', e => {
  if (!menu.contains(e.target)) {
    menu.classList.remove('abierto');
  }
});
```

Sin `contains`, una comprobación `e.target !== menu` fallaría si el click cae en un elemento hijo del menú (p. ej. un `<a>` dentro del `<ul>`), cerrando el menú cuando no debe.

## Receta: verificar que un elemento está montado en el documento

Un elemento puede existir en memoria (creado con `createElement`) sin estar insertado en el árbol del documento activo. `document.contains` permite comprobarlo:

```js
const el = document.createElement('div');
document.contains(el);  // → false  (aún no insertado)

document.body.appendChild(el);
document.contains(el);  // → true   (ya en el árbol)
```

Útil antes de operar sobre un elemento que puede haber sido removido entre llamadas asíncronas:

```js
async function actualizarUI(elemento) {
  const datos = await fetchDatos();

  if (!document.contains(elemento)) return;  // fue removido mientras esperábamos
  elemento.textContent = datos.nombre;
}
```

## Receta: modal que se cierra al click fuera, con listener de un solo uso

```js
function abrirModal(modal) {
  modal.classList.add('visible');

  function cerrarSiAfuera(e) {
    if (!modal.contains(e.target)) {
      modal.classList.remove('visible');
      document.removeEventListener('click', cerrarSiAfuera);
    }
  }

  // Timeout para evitar que el mismo click que abre el modal lo cierre inmediatamente
  setTimeout(() => {
    document.addEventListener('click', cerrarSiAfuera);
  }, 0);
}
```

## Comparación con `closest`

| Operación | Método | Dirección | Resultado |
|---|---|---|---|
| ¿Está `B` dentro de `A`? | `A.contains(B)` | `A` hacia `B` (hacia abajo) | `boolean` |
| ¿Cuál es el ancestro de `B` que cumple X? | `B.closest(selector)` | `B` hacia arriba | `Element` o `null` |

`contains` responde una pregunta binaria; `closest` devuelve una referencia al nodo encontrado.

## Cómo funciona por dentro

> [!info]
> La especificación define `contains` como equivalente a comprobar si `otroNodo` pertenece al subárbol cuya raíz es `nodo`. Internamente los motores implementan esto recorriendo la cadena de `parentNode` de `otroNodo` hasta encontrar `nodo` o llegar a `null`. El coste es O(profundidad) en el peor caso, pero para árboles de interfaz de usuario la profundidad es pequeña (10–30 niveles) y la operación es prácticamente constante.
>
> La comprobación `nodo.contains(nodo)` devuelve `true` sin recorrido porque la especificación lo exige explícitamente: un nodo se contiene a sí mismo.

> [!tip] Buenas prácticas
> - Usar `document.contains(el)` como guardia antes de operar sobre un elemento que pudo haber sido removido en código asíncrono.
> - En listeners de "click fuera", registrar el listener en `document` (no en `window`) para cubrir exactamente el árbol del documento sin capturar clicks en barras del navegador o extensiones.
> - Combinar `contains` con `removeEventListener` o `{ once: true }` para limpiar el listener cuando ya no hace falta.

> [!warning] Errores comunes
> - Usar `e.target !== menu` en lugar de `!menu.contains(e.target)`: clicks en elementos hijos del menú cierran el menú incorrectamente.
> - Registrar el listener de "click fuera" sin el `setTimeout(0)`: el mismo evento de click que activó `abrirModal` propaga a `document` y cierra el modal inmediatamente.
> - Asumir que `contains` comprueba containment visual (por CSS, `overflow: hidden`, `z-index`): solo comprueba relación en el árbol DOM, no en la presentación visual.
> - Pasar `null`: devuelve `false` sin lanzar error, pero puede enmascarar un `querySelector` fallido upstream; verificar antes.

## Notas relacionadas

- [[index | Recorrer el DOM]]
- [[04 closest]]
- [[01 parentNode y parentElement]]
