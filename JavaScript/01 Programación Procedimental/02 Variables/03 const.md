---
title: const — Binding inmutable con ámbito de bloque
aliases:
  - const
  - declaración const
tags:
  - javascript
  - api/concepto
  - variables
objeto: global
tipo: concepto
draft: false
---

# const

> [!definicion]
> `const` declara un **binding inmutable** con ámbito de bloque: el identificador no puede reasignarse tras la inicialización. Requiere un valor en la declaración. No congela el contenido del objeto o array al que apunta — solo hace que la **referencia** no pueda cambiar.

```js
const PI = 3.14159;
PI = 3; // TypeError: Assignment to constant variable.

const usuario = { nombre: "Ana" };
usuario.nombre = "Luis"; // OK — se muta la propiedad, no la referencia
usuario = {};            // TypeError — reasignación del binding
```

## Ámbito de bloque

Igual que `let`: confinada al bloque `{}` más cercano, en TDZ hasta la declaración, no re-declarable en el mismo ámbito, no adjunta a `window`.

```js
{
  const MAX = 100;
  console.log(MAX); // 100
}
console.log(MAX); // ReferenceError
```

## Inicialización obligatoria

`const` exige valor en la declaración. Separar declaración e inicialización es un `SyntaxError`.

```js
const x;       // SyntaxError: Missing initializer in const declaration
x = 5;

const y = 5;   // correcto
```

## `const` con objetos y arrays — la referencia, no el contenido

`const` fija la **referencia** almacenada en el binding, no el objeto al que apunta. Las propiedades del objeto y los elementos del array son completamente mutables.

```js
const colores = ["rojo", "verde"];
colores.push("azul");        // OK
colores[0] = "amarillo";     // OK
console.log(colores);        // ["amarillo", "verde", "azul"]

colores = ["nuevo"];         // TypeError — reasignación del binding
```

```js
const config = { debug: false, version: 1 };
config.debug = true;         // OK — mutación de propiedad
config.env = "prod";         // OK — nueva propiedad
delete config.version;       // OK — eliminar propiedad
config = {};                 // TypeError
```

## `Object.freeze()` para inmutabilidad real (shallow)

Para prevenir mutaciones en el primer nivel del objeto:

```js
const ajustes = Object.freeze({ tema: "oscuro", idioma: "es" });
ajustes.tema = "claro";  // silencioso en modo no-estricto, TypeError en strict mode
console.log(ajustes.tema); // "oscuro"
```

`Object.freeze()` es *shallow*: las propiedades que son objetos anidados siguen siendo mutables.

```js
const datos = Object.freeze({
  usuario: { nombre: "Ana" } // este objeto NO está congelado
});
datos.usuario.nombre = "Luis"; // OK — freeze no es recursivo
```

Para inmutabilidad profunda se requiere una función `deepFreeze` recursiva o una librería como Immer.

## Cómo funciona por dentro

El motor almacena el binding de `const` en el *Lexical Environment Record* con el flag `[[Writable]]: false` en la binding. Intentar escribir sobre él activa la comprobación interna `SetMutableBinding` que, al detectar el flag, lanza `TypeError`. El objeto al que apunta la referencia vive en el heap y no está afectado por este flag.

## Cuándo usar `const`

`const` es la opción **por defecto** en código moderno: comunica que el binding no cambiará, facilita el razonamiento sobre el flujo de datos y permite optimizaciones al motor. Se cambia a `let` únicamente cuando la lógica exige reasignar el binding (contadores, acumuladores, resultados de `switch`, etc.).

```js
// Preferir:
const lista = obtenerUsuarios();
const total = lista.length;

// Solo si hay reasignación real:
let intentos = 0;
while (intentos < 3) { intentos++; }
```

> [!warning] Errores comunes
> **Confundir inmutabilidad de binding con inmutabilidad de valor:** el error más frecuente es asumir que `const obj = {}` hace que el objeto sea inmutable. El binding no cambia, pero el objeto sí. Para garantizar inmutabilidad usar `Object.freeze()`.
> **`const` en bucles:** no se puede usar `const` en la inicialización de un `for` clásico (`for (const i = 0; ...)`) porque el incremento reasignaría `i`. Sí es válido en `for...of` y `for...in`, donde cada iteración crea un nuevo binding.
> ```js
> for (const item of [1, 2, 3]) {
>   console.log(item); // 1, 2, 3 — nuevo binding por iteración
> }
> ```

> [!tip] Buenas prácticas
> Declarar con `const` siempre que sea posible. Solo upgradar a `let` ante reasignación real. Usar `Object.freeze()` cuando la inmutabilidad del valor también importe (configuraciones, constantes de dominio). Documentar con un comentario si se espera que el objeto no mute aunque no esté congelado.

## Notas relacionadas

- [[01 var]]
- [[02 let]]
- [[04 Hoisting]]
- [[05 Temporal Dead Zone]]
- [[06 Ámbito (scope)/index|Ámbito (scope)]]
