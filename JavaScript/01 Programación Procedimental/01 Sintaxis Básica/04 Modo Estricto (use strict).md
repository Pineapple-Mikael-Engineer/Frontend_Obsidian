---
title: Modo Estricto — use strict
aliases:
  - use strict
  - strict mode
  - modo estricto JavaScript
tags:
  - javascript
  - api/concepto
  - sintaxis
objeto: global
tipo: concepto
draft: false
order: 4
---

# Modo Estricto (`use strict`)

> [!definicion]
> El **modo estricto** (*strict mode*) es un subconjunto más seguro de JavaScript, introducido en ES5, que convierte errores silenciosos en excepciones explícitas, prohíbe sintaxis problemática y desactiva características del lenguaje que dificultan la optimización del motor. Se activa con la directiva `"use strict"` — un literal de string al inicio de un archivo o función, no una sentencia sino una *directive prologue* reconocida por el parser.

```js
"use strict";

x = 10;  // → ReferenceError: x is not defined
          // En modo permisivo habría creado una variable global silenciosa
```

## Cómo activar el modo estricto

### A nivel de archivo (script)

```js
"use strict";  // debe ser la PRIMERA sentencia del archivo (antes de cualquier código)

// Todo el archivo se ejecuta en modo estricto
function suma(a, b) { return a + b; }
```

### A nivel de función

```js
function validar(datos) {
  "use strict";  // solo esta función (y sus anidadas) es estricta
  // código estricto aquí
}

// El scope externo puede no ser estricto
var x = 10; // no hay error aquí fuera
```

### Módulos ES6 — siempre estrictos

```js
// archivo utils.mjs o <script type="module">
export function calcular(n) {
  // Este código es SIEMPRE estricto, sin necesidad de "use strict"
  y = 5;  // → ReferenceError: y is not defined
}
```

### Clases ES6 — siempre estrictas

```js
class Contador {
  constructor() {
    z = 0;  // → ReferenceError: z is not defined
            // El cuerpo de una clase es siempre estricto
  }
}
```

> [!info]
> En la práctica moderna, cualquier código que usa módulos ES6 (`import`/`export`) o clases ya opera en modo estricto implícitamente. La directiva `"use strict"` explícita sigue siendo necesaria en scripts clásicos (`<script>` sin `type="module"`).

## Qué cambia en modo estricto

### 1. Asignación a variables no declaradas → `ReferenceError`

```js
"use strict";
contador = 0;  // → ReferenceError: contador is not defined
               // Modo permisivo: crea `window.contador` silenciosamente
```

### 2. `this` en funciones normales → `undefined`

```js
"use strict";

function mostrarThis() {
  console.log(this);
}

mostrarThis();         // → undefined  (no el objeto global)
mostrarThis.call(null); // → null       (se preserva el this pasado)

// Modo permisivo: mostrarThis() → window (en navegador) / global (en Node)
```

Esto es crítico para callbacks: en modo estricto, perder el contexto de `this` falla ruidosamente en lugar de operar sobre `window`.

### 3. Parámetros duplicados → `SyntaxError`

```js
"use strict";

function f(a, a) {  // → SyntaxError: Duplicate parameter name not allowed in this context
  return a;
}

// Modo permisivo: el segundo `a` silencia al primero (comportamiento confuso)
```

### 4. `with` → `SyntaxError`

```js
"use strict";

with (Math) {         // → SyntaxError: Strict mode code may not include a with statement
  console.log(sqrt(4));
}

// `with` altera la cadena de scope y hace imposible la optimización estática
```

### 5. `delete` sobre variables → `SyntaxError`

```js
"use strict";

var x = 1;
delete x;   // → SyntaxError: Delete of an unqualified identifier in strict mode

// Modo permisivo: `delete x` retorna false silenciosamente (no puede borrar variables)
delete undefined;  // → SyntaxError también
```

### 6. Acceso a `arguments.callee` → `TypeError`

```js
"use strict";

function factorial(n) {
  if (n <= 1) return 1;
  return n * arguments.callee(n - 1);  // → TypeError: 'caller', 'callee', and 'arguments' properties may not be accessed
}

// Alternativa correcta: usar el nombre de la función directamente
function factorial(n) {
  "use strict";
  if (n <= 1) return 1;
  return n * factorial(n - 1);  // ✓
}
```

### 7. Palabras reservadas futuras como identificadores → `SyntaxError`

```js
"use strict";

var implements = 1;  // → SyntaxError
var interface  = 2;  // → SyntaxError
var let        = 3;  // → SyntaxError
var static     = 4;  // → SyntaxError
var package    = 5;  // → SyntaxError

// Lista completa de palabras reservadas en strict mode:
// implements, interface, let, package, private, protected, public, static, yield
```

### 8. Asignación a propiedades de solo lectura → `TypeError`

```js
"use strict";

const obj = Object.freeze({ x: 1 });
obj.x = 2;  // → TypeError: Cannot assign to read only property 'x'
            // Modo permisivo: la asignación falla silenciosamente (obj.x sigue siendo 1)

NaN = 1;    // → TypeError: Cannot assign to read only property 'NaN'
undefined = "valor"; // → TypeError
```

### 9. `eval` y `arguments` no reasignables

```js
"use strict";

eval = function() {};   // → SyntaxError: Unexpected eval or arguments in strict mode
arguments++;            // → SyntaxError

// En modo permisivo, reasignar `eval` o `arguments` tiene comportamientos indefinidos
```

## Tabla resumen de cambios

| Comportamiento | Modo permisivo | Modo estricto |
|---|---|---|
| Variable no declarada | Crea global silenciosa | `ReferenceError` |
| `this` en función sin contexto | `window` / `global` | `undefined` |
| Parámetros duplicados | Silencioso (el último gana) | `SyntaxError` |
| `with` | Permitido | `SyntaxError` |
| `delete` sobre variable | `false` silencioso | `SyntaxError` |
| `arguments.callee` | Permitido | `TypeError` |
| Palabras reservadas futuras | Permitidas como identificadores | `SyntaxError` |
| Asignación a read-only | Fallo silencioso | `TypeError` |

## Cómo funciona por dentro

El parser de JS activa un **flag booleano de modo estricto** al encontrar `"use strict"`. Este flag acompaña al AST (Abstract Syntax Tree) generado para ese ámbito. El intérprete (y el compilador JIT) consultan el flag en cada operación que tiene comportamiento diferente según el modo:

- Al encontrar una asignación de variable (`LHS`), comprueba el flag antes de decidir si crear una propiedad global o lanzar `ReferenceError`.
- Al ejecutar una llamada de función, si el flag está activo, establece `this` como `undefined` en lugar del objeto global.
- El compilador JIT puede optimizar más agresivamente el código estricto porque elimina ambigüedades como `with` (que haría imposible determinar el scope en tiempo de compilación) y `arguments.callee` (que impediría inlining).

```js
// El parser produce un AST diferente para:
"use strict";
function f() { /* … */ }
// vs.
function f() { /* … */ }  // sin use strict

// El nodo FunctionDeclaration en el AST tiene strict: true o false
// según el contexto donde fue parseada
```

> [!tip] Buenas prácticas
> - Usar siempre módulos ES6 (`type="module"`) en código nuevo — el modo estricto es automático y el scope del módulo evita la polución del global.
> - En scripts clásicos (legado), añadir `"use strict"` al inicio de cada archivo, no solo de cada función.
> - No mezclar código estricto y no-estricto en el mismo script cuando sea evitable — el comportamiento de `this` y de los errores varía y puede ser difícil de razonar.
> - Aprovechar que el modo estricto hace visibles errores que el modo permisivo oculta: durante desarrollo, los `ReferenceError` y `TypeError` son señales valiosas.

> [!warning] Errores comunes
> - Poner `"use strict"` después de otra sentencia: deja de ser una directive prologue y no activa el modo estricto (el parser ya pasó el punto de reconocimiento).
> - Concatenar un script estricto con uno no-estricto: si el script estricto va primero, su `"use strict"` afecta al resultado concatenado; si va segundo, no. Las herramientas de bundling resuelven esto envolviendo cada módulo en su propio scope.
> - Asumir que todas las funciones dentro de un archivo estricto son estrictas: sí lo son, pero solo si el archivo tiene `"use strict"` al inicio, no una función anidada concreta.
> - Confundir "modo estricto" con "tipado estricto" (TypeScript): son conceptos distintos. El modo estricto de JS no verifica tipos.

## Notas relacionadas

- [[01 Declaraciones y Expresiones]]
- [[02 Punto y Coma]]
- [[02 Variables/index | Variables (var, let, const)]]
- [[02 Programación Orientada a Objetos/04 this/index | this en JavaScript]]
- [[10 Módulos y Organización/01 Módulos ES6/index | Módulos ES6]]
- [[02 Programación Orientada a Objetos/03 Clases/index | Clases]]
- [[11 Manejo de Errores y Depuración/01 Tipos de Errores/02 ReferenceError | ReferenceError]]
- [[11 Manejo de Errores y Depuración/01 Tipos de Errores/03 TypeError | TypeError]]
