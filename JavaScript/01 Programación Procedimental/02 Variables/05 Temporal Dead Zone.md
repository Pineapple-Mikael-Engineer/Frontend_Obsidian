---
title: Temporal Dead Zone — La zona de acceso prohibido
aliases:
  - TDZ
  - Temporal Dead Zone
  - zona muerta temporal
tags:
  - javascript
  - api/concepto
  - variables
tipo: concepto
draft: false
order: 5
---

# Temporal Dead Zone

> [!definicion]
> La **Temporal Dead Zone** (TDZ) es el período entre el inicio de un bloque y la línea de declaración de una variable `let`, `const` o `class`, durante el cual la variable **existe en el Environment Record pero no puede accederse**. Cualquier lectura o escritura dentro de ese período lanza `ReferenceError`.

```js
{
  // ← TDZ comienza aquí para `x`
  console.log(x); // ReferenceError: Cannot access 'x' before initialization
  let x = 5;      // ← TDZ termina aquí
  console.log(x); // 5
}
```

## Por qué existe la TDZ

La TDZ es un diseño deliberado de ES6 para eliminar el comportamiento confuso de `var`, donde una variable podía leerse antes de declararse devolviendo `undefined` sin ningún error. Con `let`/`const`, el motor detecta el uso prematuro y lanza un `ReferenceError` descriptivo, haciendo el bug visible inmediatamente.

```js
// Con var — bug silencioso:
console.log(cuenta); // undefined (no da error, difícil de detectar)
var cuenta = 0;

// Con let — error descriptivo:
console.log(total); // ReferenceError: Cannot access 'total' before initialization
let total = 0;
```

## TDZ y `typeof`

`typeof` normalmente devuelve `"undefined"` para identificadores no declarados sin lanzar error. Sin embargo, **con variables en TDZ sí lanza `ReferenceError`**:

```js
// Variable no declarada — typeof es seguro:
console.log(typeof noExiste); // "undefined"  ← sin error

// Variable en TDZ — typeof lanza error:
console.log(typeof enTDZ);   // ReferenceError
let enTDZ = 42;
```

Esta diferencia es relevante al usar `typeof` como guardia para detectar si una variable existe: el patrón solo es seguro con `var` o con identificadores completamente no declarados.

## TDZ en parámetros por defecto

Los parámetros de una función se evalúan en orden de izquierda a derecha en su propio ámbito. Un parámetro puede referenciar a un parámetro anterior, pero no a uno posterior (que aún está en TDZ).

```js
// OK — b referencia a a, que ya fue evaluado:
function suma(a, b = a * 2) {
  return a + b;
}
suma(3); // 9  (b = 3 * 2 = 6)

// Error — a referencia a b, que aún está en TDZ:
function error(a = b, b = 1) {
  return a + b;
}
error(); // ReferenceError: Cannot access 'b' before initialization
```

## TDZ en clases

Las declaraciones de clase siguen las mismas reglas que `let`/`const`: están en TDZ hasta su línea de declaración.

```js
const instancia = new Forma(); // ReferenceError

class Forma {
  constructor() { this.tipo = "círculo"; }
}
```

## Ejemplo: TDZ en bloques anidados

La TDZ se calcula por bloque. Una variable en un bloque exterior que hace *shadowing* en uno interior crea una TDZ en el bloque interior desde su apertura hasta su declaración.

```js
let valor = "exterior";

{
  // ← TDZ de la variable local `valor` comienza aquí
  console.log(valor); // ReferenceError (shadowing: la outer queda oculta)
  let valor = "interior"; // ← TDZ termina
  console.log(valor); // "interior"
}

console.log(valor); // "exterior"
```

## Cómo funciona por dentro

Cuando el motor entra en un bloque, instancia los bindings de `let`/`const` en el *Lexical Environment Record* con el estado especial **`[[BindingStatus]]: uninitialized`**. La operación `GetBindingValue` en la especificación comprueba este estado: si es `uninitialized`, lanza `ReferenceError` sin importar si la variable fue declarada en el código fuente.

La línea de declaración (con o sin valor inicial) ejecuta `InitializeBinding`, que actualiza el estado a `initialized` y almacena el valor. A partir de ese punto, la variable es accesible normalmente.

```
Inicio de bloque:
  x → uninitialized  ← cualquier acceso aquí → ReferenceError

Línea: let x = 5;
  x → 5              ← acceso permitido a partir de aquí
```

> [!warning] Errores comunes
> **Shadowing accidental con TDZ:** redeclarar con `let` en un bloque anidado oculta la variable del ámbito exterior y crea una TDZ al inicio del bloque interno. Leer la variable "exterior" dentro del bloque antes de la declaración interna lanza `ReferenceError`, aunque la variable exterior sí exista.
> **`typeof` como guardia:** en código que mezcla `var` y `let`, `typeof variable` no es una comprobación de existencia segura para variables `let`/`const` — lanzará error si están en TDZ.

> [!tip] Buenas prácticas
> Declarar `let`/`const` al inicio del bloque donde se usan para minimizar (o eliminar) la TDZ en la práctica. El `ReferenceError` de la TDZ es una herramienta útil: indica un bug de orden de inicialización que conviene corregir en el código, no eludir.

## Notas relacionadas

- [[01 var]]
- [[02 let]]
- [[03 const]]
- [[04 Hoisting]]
- [[06 Ámbito (scope)/index|Ámbito (scope)]]
