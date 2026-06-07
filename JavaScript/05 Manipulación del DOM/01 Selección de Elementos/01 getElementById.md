---
title: document.getElementById — Seleccionar por ID
aliases:
  - getElementById
tags:
  - javascript
  - api/metodo
  - dom
objeto: document
tipo: metodo
retorna: Element | null
muta: false
asincrono: false
draft: false
---

# `document.getElementById`

> [!definicion]
> `document.getElementById(id)` retorna el `Element` cuyo atributo `id` coincide exactamente con la cadena `id`, o `null` si no existe ninguno. Solo está disponible en `document`; no se hereda por los elementos del árbol.

```js
const btn = document.getElementById('submit-btn');

if (btn) {
  btn.disabled = true;
}
// → el botón queda deshabilitado, o no ocurre nada si el id no existe
```

## Firma

```text
document.getElementById(id: string): Element | null
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `id` | `string` | El valor exacto del atributo `id` a buscar |

**Retorna:** el primer (y único) `Element` con ese `id`, o `null`. No retorna `HTMLCollection` ni `NodeList`.

## Detalles de comportamiento

**Case-sensitive.** El `id` se compara carácter a carácter:

```js
document.getElementById('miDiv')  // busca "miDiv"
document.getElementById('midiv')  // null — no es lo mismo
```

**IDs duplicados.** El HTML válido exige IDs únicos. Con IDs duplicados el comportamiento es indefinido por la especificación; los motores actuales retornan el primer elemento en orden de documento, pero esto es un error de markup, no un contrato.

**Solo en `document`.** A diferencia de `querySelector`, no existe `element.getElementById(...)`. Para buscar dentro de un subárbol por ID se usa `elemento.querySelector('#miId')`.

## Velocidad

Es el método de selección más rápido disponible en la Web API: el motor mantiene un mapa interno `id → Element` (estructura hash) que se actualiza al parsear el HTML y al modificar el DOM. La búsqueda es O(1), independiente del tamaño del árbol.

## Receta: leer el valor de un campo de formulario

```html
<input id="email" type="email" value="user@example.com" />
```

```js
const input = document.getElementById('email');

if (input) {
  const valor = input.value;          // "user@example.com"
  const esValido = input.checkValidity();
  console.log(valor, esValido);       // → "user@example.com" true
}
```

## Cómo funciona por dentro

Al parsear el HTML, el motor HTML (p. ej. Blink, Gecko) construye el árbol del DOM e indexa simultáneamente cada elemento que tiene un atributo `id` en una tabla hash `id → Element` que vive en el objeto `Document`. Las operaciones de mutación del DOM (`insertBefore`, `replaceChild`, `setAttribute`, etc.) actualizan esa tabla. `getElementById` simplemente consulta la tabla: sin recorrido de árbol, sin comparaciones en cascada.

## Alternativa moderna

`document.querySelector('#miId')` produce el mismo resultado y acepta selectores más expresivos, pero pasa por el motor de selectores CSS, que es más lento que la consulta directa a la tabla hash. La diferencia es despreciable para la mayoría de casos; se justifica `getElementById` en código de alto rendimiento o en abstracciones que ya usan el `id` como clave.

> [!tip] Buenas prácticas
> - Comprobar siempre que el resultado no es `null` antes de acceder a propiedades del elemento.
> - No asumir que `getElementById` está disponible en cualquier contexto: dentro de un Web Worker o un módulo de Node.js no existe `document`.
> - Usar `getElementById` cuando el selector es literalmente un `id` y la legibilidad no se ve afectada; usar `querySelector` cuando se necesita encadenamiento de selectores.

> [!warning] Errores comunes
> - Pasar `'#miId'` con el `#` incluido: `getElementById('#app')` busca un elemento con `id` literal `"#app"`, que no existe. El `#` es sintaxis de selector CSS, no de ID. Correcto: `getElementById('app')`.
> - Llamar al método antes de que el elemento exista en el DOM (script en `<head>` sin `defer` o sin esperar `DOMContentLoaded`): retorna `null`.
> - Asumir unicidad sin validar el HTML: con IDs duplicados el resultado es impredecible.

## Notas relacionadas

- [[index | Selección de Elementos]]
- [[03 querySelector]]
- [[05 HTMLCollection vs NodeList]]
