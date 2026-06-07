---
title: Element.closest — Buscar ancestro por selector CSS
aliases:
  - closest
  - Element.closest
tags:
  - javascript
  - api/metodo
  - dom
objeto: Element
tipo: metodo
retorna: Element | null
muta: false
asincrono: false
draft: false
---

# `Element.closest`

> [!definicion]
> `elemento.closest(selector)` recorre los ancestros del elemento hacia la raíz del documento —comenzando por el propio elemento— y devuelve el **primero** que coincide con el selector CSS. Devuelve `null` si ningún ancestro (incluido el elemento) coincide. Es la operación inversa a `querySelector`: donde `querySelector` baja hacia los descendientes, `closest` sube hacia los ancestros.

```js
const btn = document.querySelector('button.guardar');

// Encontrar el formulario que contiene el botón, sin importar la profundidad
const form = btn.closest('form');
form?.id;   // → "login-form"

// Si el elemento mismo coincide con el selector, se devuelve el propio elemento
btn.closest('button');  // → el propio <button>

// Sin ancestro que coincida
btn.closest('.no-existe');  // → null
```

## Firma

```text
Element.closest(selector: string): Element | null
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `selector` | `string` | Selector CSS válido; admite cualquier selector que acepte `querySelector` |

**Retorna:** el primer ancestro (o el propio elemento) que coincide, o `null`.

## Caso de uso estrella: delegación de eventos

La delegación de eventos consiste en registrar un único listener en el padre y detectar el elemento relevante desde `e.target`. `closest` convierte esto en un patrón fiable aunque el click caiga en un elemento hijo del target real:

```js
// HTML: <ul class="lista"> <li class="item"><button>Borrar</button></li> </ul>

document.querySelector('.lista').addEventListener('click', e => {
  // El click puede ser en el <button> interior, no en el <li> directamente
  const item = e.target.closest('.item');
  if (!item) return;   // click fuera de un .item

  item.remove();
});
```

Sin `closest`, un click en el `<button>` interior haría que `e.target` fuera el botón y la comprobación `e.target.classList.contains('item')` fallara.

## Receta: formulario contenedor de un input

```js
const input = document.getElementById('correo');
const form = input.closest('form');

form?.addEventListener('submit', manejarEnvio);
```

Más robusto que `input.parentElement.parentElement`: si se añade un `<fieldset>` intermedio entre el `<input>` y el `<form>`, el encadenamiento de `parentElement` se rompe; `closest` sigue funcionando.

## Receta: modal más cercano

```js
const botonCerrar = document.querySelector('.btn-cerrar');

botonCerrar.addEventListener('click', () => {
  const modal = botonCerrar.closest('[role="dialog"]');
  modal?.classList.remove('visible');
  modal?.setAttribute('aria-hidden', 'true');
});
```

El selector de atributo `[role="dialog"]` permite trabajar con la semántica ARIA sin depender de clases CSS.

## Receta: detectar si un elemento está dentro de una tabla

```js
const celda = document.querySelector('td');

if (celda.closest('table.datos-principales')) {
  // es descendiente de esa tabla concreta
}
```

## Cómo funciona por dentro

> [!info]
> Al invocar `closest(selector)`, el motor:
> 1. Comprueba si el elemento actual coincide con el selector usando el mismo motor CSS que alimenta `querySelector`.
> 2. Si coincide, lo devuelve.
> 3. Si no, sube al `parentElement` y repite desde el paso 1.
> 4. Si `parentElement` es `null` (se alcanzó la raíz o el elemento está en un `DocumentFragment`), devuelve `null`.
>
> El coste es proporcional a la profundidad del árbol hasta el ancestro buscado multiplicado por el coste de evaluar el selector. Para selectores simples (clase, id, tag) el coste por nivel es O(1); el ancestro suele estar pocos niveles arriba, así que la operación es prácticamente constante en DOM normales.

> [!tip] Buenas prácticas
> - Preferir `closest` sobre `parentElement` encadenado cuando la profundidad del ancestro puede variar: el selector CSS hace el código más declarativo y resistente a cambios de markup.
> - Combinar con optional chaining para evitar `TypeError` cuando el resultado es `null`: `el.closest('.modal')?.remove()`.
> - En delegación de eventos, poner la comprobación `if (!item) return` inmediatamente después de `closest`: así el handler no procesa clicks en zonas sin el ancestro esperado.

> [!warning] Errores comunes
> - Confundir la dirección: `closest` busca hacia **arriba** (ancestros), no hacia abajo (descendientes). Para buscar descendientes usar `querySelector`.
> - Asumir que `closest` no incluye el elemento actual: sí lo incluye. `el.closest('div')` devuelve el propio `el` si es un `<div>`.
> - Usar `closest` sobre `null`: si la referencia inicial es `null` (por un `querySelector` fallido), el acceso a `.closest` lanza `TypeError`. Verificar siempre la referencia base.
> - Pasar un selector inválido: lanza `SyntaxError`. El selector se valida en el momento de la llamada.

## Notas relacionadas

- [[index | Recorrer el DOM]]
- [[01 parentNode y parentElement]]
- [[05 contains]]
- [[01 Selección de Elementos/03 querySelector | querySelector]]
