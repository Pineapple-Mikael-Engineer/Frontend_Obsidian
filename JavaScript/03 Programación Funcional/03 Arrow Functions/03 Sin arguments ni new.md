---
title: Arrow Functions — Sin arguments, new, super ni generadores
aliases:
  - arguments en arrows
  - arrow constructor
  - sin new arrow
  - limitaciones arrow functions
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
---

# Sin arguments ni new

> [!definicion]
> Las arrow functions carecen de cuatro capacidades que sí tienen las funciones regulares: **objeto `arguments`** propio, capacidad de actuar como **constructora** con `new`, acceso a **`super`** mediante `[[HomeObject]]`, y sintaxis de **función generadora**. Estas ausencias no son limitaciones accidentales — derivan de que las arrows son funciones ordinarias sin los slots internos (`[[Construct]]`, `[[HomeObject]]`) que habilitan esas características.

```js
// arguments — no existe en arrow
const f = () => arguments; // ReferenceError si está en módulo ES
                            // o hereda arguments del scope exterior si lo hay

// new — error en tiempo de ejecución
const Cls = () => {};
new Cls(); // TypeError: Cls is not a constructor

// Generadora — error de sintaxis
const gen = *() => {}; // SyntaxError
```

## Sin objeto `arguments`

Las funciones regulares crean automáticamente un objeto `arguments` en cada invocación. Las arrows no — si se escribe `arguments` dentro de una arrow, el motor busca en el scope léxico exterior:

```js
function exterior() {
  // arguments aquí es el de exterior()
  const flecha = () => {
    console.log(arguments); // hereda arguments de exterior
  };
  flecha(99, 88); // imprime los args de exterior(), no [99, 88]
}
exterior(1, 2, 3); // → Arguments [1, 2, 3]

// En un módulo ES sin función exterior:
const sola = () => arguments; // ReferenceError: arguments is not defined
```

Este comportamiento puede generar bugs sutiles cuando se migra una función regular a arrow y se deja `arguments` en el cuerpo.

### Alternativa: rest params

Los rest params son la solución idiomática y funcionan perfectamente en arrows. A diferencia de `arguments`, producen un `Array` real (con `map`, `filter`, etc.) y soportan destructuring:

```js
// Con rest params — Array real, no objeto Arguments
const sumar  = (...nums)       => nums.reduce((acc, n) => acc + n, 0);
const unir   = (sep, ...items) => items.join(sep);
const log    = (nivel, ...msg) => console.log(`[${nivel}]`, ...msg);

sumar(1, 2, 3, 4, 5);           // → 15
unir(" | ", "a", "b", "c");     // → "a | b | c"
log("INFO", "Servidor", "listo"); // → [INFO] Servidor listo
```

| Característica | `arguments` | rest params (`...args`) |
|---|---|---|
| Tipo | Objeto Arguments (array-like) | `Array` real |
| Métodos de array | No directamente | Sí (`map`, `filter`, `reduce`) |
| Disponible en arrows | No (hereda o ReferenceError) | Sí |
| Captura solo el resto | No — todos los args siempre | Sí — desde la posición indicada |
| Soporte en modo estricto | Restringido | Sin restricciones |

## Sin new — las arrows no son constructoras

`new` requiere que la función tenga el slot interno `[[Construct]]`. Las arrows no lo tienen. Intentar usar `new` con una arrow lanza `TypeError` en tiempo de ejecución:

```js
const Punto = (x, y) => ({ x, y });
const p = new Punto(1, 2); // TypeError: Punto is not a constructor

// La función regular equivalente sí puede usarse con new:
function PuntoRegular(x, y) {
  this.x = x;
  this.y = y;
}
const q = new PuntoRegular(1, 2); // → { x: 1, y: 2 }
```

Tampoco tienen la propiedad `prototype`, que `new` necesita para enlazar el prototipo del objeto creado:

```js
const flecha   = () => {};
function normal() {}

"prototype" in flecha; // → false
"prototype" in normal; // → true
```

## Sin super

`super` en métodos de clase depende del slot `[[HomeObject]]`, que se asigna al definir el método en el objeto o clase. Las arrows no reciben este slot aunque estén escritas dentro de un método. Si la arrow está **dentro** de un método regular que sí tiene `[[HomeObject]]`, puede usar `super` de forma indirecta a través del scope léxico — pero la arrow en sí no lo posee:

```js
class Animal {
  hablar() { return "..."; }
}

class Perro extends Animal {
  // Método regular: tiene [[HomeObject]] → super funciona
  hablarRegular() {
    return super.hablar() + " guau";
  }

  // Campo arrow: no tiene [[HomeObject]] → TypeError si se usa super
  hablarFlecha = () => {
    // super.hablar() aquí lanzaría TypeError
    return "guau";
  };
}
```

## Sin función generadora

La sintaxis `function*` no se puede combinar con arrows. No existe una forma de definir una función generadora con `=>`:

```js
// Todos son errores de sintaxis:
const gen1 = *() => { yield 1; };
const gen2 = () => * { yield 1; };
const gen3 = function*() => { yield 1; };

// La única forma de generadora es function*:
function* generadora() { yield 1; yield 2; }
const gen4 = function*() { yield 1; yield 2; };
```

## Resumen de limitaciones

| Capacidad | Función regular | Arrow function |
|---|---|---|
| Objeto `arguments` propio | Sí | No — hereda del scope o ReferenceError |
| `new` (constructora) | Sí | No — TypeError en runtime |
| `super` con `[[HomeObject]]` | Sí (en métodos) | No |
| Sintaxis generadora `function*` | Sí | No — SyntaxError |
| Propiedad `prototype` | Sí | No |

## Cómo funciona por dentro

El spec de ECMAScript distingue las _Arrow Function Exotic Objects_ de las funciones ordinarias por la ausencia de tres slots internos:

- **`[[Construct]]`** — requerido por `new`. Sin él, el motor lanza `TypeError` al intentar construir.
- **`[[HomeObject]]`** — requerido por `super`. Se asigna durante la evaluación de un `MethodDefinition` en clase u objeto literal; las arrows no pasan por esa ruta de evaluación.
- **El frame de ejecución de una arrow** no inicializa la variable local `arguments` — a diferencia de las funciones regulares, donde el motor crea el objeto `Arguments` y lo enlaza en el `FunctionEnvironmentRecord`.

El resultado es un objeto función más ligero que no incurre en el coste de inicializar `arguments`, `prototype` ni los mecanismos de construcción.

> [!tip] Buenas prácticas
> - Sustituye siempre `arguments` por rest params cuando migres funciones regulares a arrows — el código es más explícito y `args` es un `Array` real.
> - Si necesitas una función constructora ligera, usa una función regular o, preferiblemente, `class` — no intentes forzar arrows para ese rol.
> - Para métodos que necesitan `super`, declara siempre un método de clase regular, no un campo arrow.

> [!warning] Errores comunes
> - Usar `arguments` en una arrow asumiendo que se refiere a los args de la arrow — se hereda del scope exterior o es `ReferenceError`.
> - Escribir `new MiArrow()` esperando que funcione — el error aparece en runtime, no en la definición, lo que puede dificultar la depuración.
> - Definir métodos de subclase como campos arrow y usar `super` dentro: el `[[HomeObject]]` no existe en el campo y `super` lanza error.
> - Asumir que `...args` y `arguments` son equivalentes: `arguments` incluye todos los argumentos sin posibilidad de capturar solo "el resto"; rest params captura a partir de una posición específica.

## Notas relacionadas

- [[01 Sintaxis y Return Implícito]] — sintaxis general y rest params en parámetros
- [[02 Sin this Propio]] — la otra limitación semántica principal de las arrows
- [[04 Cuándo Usarlas]] — guía de decisión completa entre arrow y función regular
