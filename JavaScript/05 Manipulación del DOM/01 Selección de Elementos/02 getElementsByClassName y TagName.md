---
title: getElementsByClassName y getElementsByTagName — Colecciones live por clase y tag
aliases:
  - getElementsByClassName
  - getElementsByTagName
tags:
  - javascript
  - api/metodo
  - dom
objeto: Element
tipo: metodo
retorna: HTMLCollection
muta: false
asincrono: false
draft: false
order: 2
---

# `getElementsByClassName` y `getElementsByTagName`

> [!definicion]
> `element.getElementsByClassName(clases)` retorna una `HTMLCollection` **live** con todos los descendientes del elemento que poseen las clases indicadas. `element.getElementsByTagName(tag)` retorna una `HTMLCollection` **live** con todos los descendientes con el nombre de etiqueta dado. Ambos métodos están disponibles en `document` y en cualquier `Element`.

```js
// Por clase
const activos = document.getElementsByClassName('activo');
// → HTMLCollection live con todos los elementos que tienen class "activo"

// Múltiples clases (AND): el elemento debe tener TODAS
const dest = document.getElementsByClassName('card destacado');

// Por tag
const parrafos = document.getElementsByTagName('p');
const todos    = document.getElementsByTagName('*');   // todos los elementos
```

## Firmas

```text
element.getElementsByClassName(clases: string): HTMLCollection
element.getElementsByTagName(tag: string): HTMLCollection
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `clases` | `string` | Una o varias clases separadas por espacio (AND lógico) |
| `tag` | `string` | Nombre de etiqueta HTML (`"p"`, `"div"`, `"*"` para todos) |

**Retornan:** `HTMLCollection` live — una vista activa del árbol del DOM, no una copia.

## La naturaleza live: ventaja y trampa

Una `HTMLCollection` live refleja el estado actual del DOM en cada acceso a sus miembros. Si se agrega o elimina un elemento que coincide con el criterio, la colección se actualiza automáticamente sin volver a llamar al método.

```js
const items = document.getElementsByClassName('item');
console.log(items.length);   // → 3

document.querySelector('.item').remove();
console.log(items.length);   // → 2  (actualización automática)
```

La trampa ocurre al iterar con índice mientras se muta el DOM: la longitud cambia durante el bucle y se saltan o repiten elementos.

```js
// BUG: el bucle se salta elementos al eliminar
const items = document.getElementsByClassName('item');
for (let i = 0; i < items.length; i++) {
  items[i].remove();   // items.length decrece; i avanza → se saltan elementos
}

// Correcto: convertir a array estático primero
[...items].forEach(el => el.remove());
// o bien: iterar desde el final
for (let i = items.length - 1; i >= 0; i--) {
  items[i].remove();
}
```

## Tabla: `getElementsByClassName` vs `querySelectorAll`

| Aspecto | `getElementsByClassName` | `querySelectorAll` |
|---|---|---|
| Tipo de colección | `HTMLCollection` live | `NodeList` static |
| Sintaxis de clase | `"card"` (sin punto) | `".card"` (selector CSS) |
| Múltiples clases | `"a b"` (espacio = AND) | `".a.b"` o `".a, .b"` |
| Selectores complejos | No | Sí (descendiente, atributo, pseudo) |
| Contexto | `document` o elemento | `document` o elemento |
| Rendimiento relativo | Más rápido (índice interno) | Más lento (motor CSS) |
| `forEach` nativo | No | Sí |

## Receta: iterar con `for...of`

`HTMLCollection` es iterable con `for...of` en navegadores modernos, pero no tiene `forEach`. Para usar métodos de array se convierte:

```js
const filas = document.getElementsByTagName('tr');

// for...of — funciona directamente
for (const fila of filas) {
  fila.classList.toggle('par');
}

// forEach — requiere conversión
Array.from(filas).forEach(fila => console.log(fila.textContent));

// map, filter, reduce — también requieren conversión
const textos = [...filas].map(f => f.textContent.trim());
```

## Receta: buscar dentro de un subárbol

Ambos métodos están disponibles en cualquier elemento, no solo en `document`. Esto restringe la búsqueda a los descendientes de ese nodo:

```js
const tabla = document.getElementById('ventas');
const celdas = tabla.getElementsByTagName('td');
// → solo las <td> dentro de #ventas, no del documento completo
```

## Cómo funciona por dentro

El motor HTML (Blink, Gecko, WebKit) mantiene índices internos del árbol del DOM organizados por nombre de clase y por nombre de etiqueta. `getElementsByClassName` y `getElementsByTagName` no recorren el árbol al ser invocados; retornan un objeto `HTMLCollection` que es una **vista** sobre esos índices. Cada vez que se accede a la propiedad `length` o a un elemento por índice, la colección consulta el índice interno, que siempre está sincronizado con el estado actual del árbol. Por eso es live: no es una copia, es una referencia a la estructura interna del motor.

> [!tip] Buenas prácticas
> - Preferir `querySelectorAll` cuando se necesita una colección estática para iterar sin riesgo de mutación concurrente.
> - Usar `getElementsByClassName` o `getElementsByTagName` cuando se necesita una vista que refleje cambios futuros del DOM (p. ej. un contador de elementos activos que se actualiza solo).
> - Convertir siempre a array (`[...col]` o `Array.from(col)`) antes de cualquier iteración que modifique el DOM.

> [!warning] Errores comunes
> - Pasar el punto en `getElementsByClassName`: `getElementsByClassName('.activo')` busca la clase literal `".activo"` (con punto), no encuentra nada. El punto es sintaxis de selector CSS.
> - Asumir que `HTMLCollection` tiene `forEach`: lanza `TypeError`. Usar `for...of` o convertir a array.
> - Iterar con índice sobre una colección live mientras se muta el DOM: comportamiento impredecible.
> - `getElementsByTagName('*')` en documentos grandes puede ser costoso; preferir un selector más específico.

## Notas relacionadas

- [[index | Selección de Elementos]]
- [[05 HTMLCollection vs NodeList]]
- [[04 querySelectorAll]]
