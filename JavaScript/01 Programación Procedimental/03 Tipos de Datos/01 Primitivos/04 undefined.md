---
title: undefined — Ausencia de valor no inicializado
aliases:
  - undefined JS
  - tipo undefined
tags:
  - javascript
  - api/concepto
  - tipos
objeto: global
tipo: concepto
draft: false
order: 4
---

# undefined

> [!definicion]
> `undefined` es el valor que JS asigna automáticamente cuando algo **no ha sido inicializado o no existe**: variables declaradas sin valor, parámetros no pasados, propiedades ausentes en objetos, y funciones sin `return`. Solo existe un valor de tipo `undefined`.

```js
typeof undefined           // → "undefined"

let x;
console.log(x);            // → undefined

function f() {}
console.log(f());          // → undefined
```

## Cuándo aparece `undefined`

```js
// 1. Variable declarada sin inicializar
let a;
console.log(a);            // → undefined

// 2. Parámetro no pasado
function saludar(nombre) {
  console.log(nombre);
}
saludar();                 // → undefined

// 3. Función sin return (o con return vacío)
function nada() { return; }
console.log(nada());       // → undefined

// 4. Propiedad no existente en un objeto
const obj = { x: 1 };
console.log(obj.y);        // → undefined

// 5. Elemento fuera de rango en array
const arr = [1, 2, 3];
console.log(arr[10]);      // → undefined
```

## `typeof` con variables no declaradas

La ventaja clave de `typeof` sobre cualquier otro acceso es que **no lanza ReferenceError** para variables no declaradas:

```js
// Si 'foo' no está declarada:
typeof foo           // → "undefined"  (seguro)
console.log(foo)     // → ReferenceError: foo is not defined

// Patrón para detección de features (entornos sin módulos)
if (typeof window !== "undefined") {
  // código de navegador
}
```

Para `let` y `const` dentro de su TDZ (Temporal Dead Zone), `typeof` sí lanza ReferenceError.

## `undefined` vs. `null`

| Aspecto | `undefined` | `null` |
|---|---|---|
| Significado | No inicializado / no existe | Ausencia intencional de objeto |
| `typeof` | `"undefined"` | `"object"` |
| Asignado por | El motor JS automáticamente | El programador explícitamente |
| Uso tipico | Parámetro omitido, propiedad ausente | Variable que debería ser objeto pero aun no lo es |
| `== null` | `true` | `true` |
| `=== null` | `false` | `true` |

La distinción semántica es relevante en APIs: si una función retorna `null`, el diseño comunica "no hay resultado pero se buscó"; si retorna `undefined`, comunica "no aplica" o "error de uso".

## Comprobar undefined

```js
let valor;

// Opción 1: comparación estricta
valor === undefined        // → true

// Opción 2: typeof (segura para variables potencialmente no declaradas)
typeof valor === "undefined"   // → true

// Opción 3: nullish (cubre undefined Y null)
valor == null              // → true  (doble igual, cubre ambos)
```

La comprobación `valor == null` (doble igual, no triple) es el patrón idiomático para "es null o undefined a la vez", sin importar cuál de los dos sea.

> [!tip] Buenas prácticas
> - No asignar `undefined` explícitamente: usar `null` cuando se quiere indicar ausencia intencional.
> - Para detectar si una variable global existe en entornos sin módulos, usar `typeof variable !== "undefined"`.
> - Para parámetros opcionales, usar valores por defecto en la firma: `function f(x = 0)` en lugar de comprobar `x === undefined` dentro.

> [!warning] Errores comunes
> - `undefined` es falsy: actúa como `false` en condiciones, pero `undefined !== false`.
> - `obj.prop` de una propiedad inexistente devuelve `undefined` (sin error); pero `undefined.algo` lanza `TypeError`. Encadenar accesos sobre un resultado potencialmente `undefined` sin optional chaining (`?.`) provoca errores en runtime.
> - En TDZ, `typeof` sí lanza `ReferenceError` para `let`/`const` (aunque es inusual hacer ese acceso).

## Notas relacionadas

- [[05 null|null]] — ausencia intencional; diferencia semántica con `undefined`
- [[04 Conversión de Tipos/03 Truthy y Falsy|Truthy y Falsy]] — `undefined` es falsy
- [[02 typeof|typeof]] — comportamiento seguro con variables no declaradas
- [[01 Primitivos/index|Primitivos]]
