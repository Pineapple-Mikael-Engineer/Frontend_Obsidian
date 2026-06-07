---
title: Arrow Functions
aliases:
  - arrow functions
  - funciones flecha
  - fat arrow
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
---

# Arrow Functions

> [!definicion]
> Una **arrow function** es una expresión de función con sintaxis concisa (`=>`) que **no crea su propio `this`, `arguments`, `super` ni `prototype`**. Captura el `this` del ámbito léxico en el que se define — por eso no puede usarse como constructora ni como método de objeto que necesite referenciar al objeto receptor. No son simplemente funciones más cortas: tienen diferencias semánticas fundamentales respecto a las funciones regulares.

```js
// Función regular
function doble(x) { return x * 2; }

// Arrow equivalente — return implícito
const doble = x => x * 2;

// Arrow con cuerpo de bloque — return explícito obligatorio
const dividir = (a, b) => {
  if (b === 0) throw new Error("División por cero");
  return a / b;
};
```

## Función regular vs arrow — diferencias semánticas

| Característica | Función regular | Arrow function |
|---|---|---|
| `this` | Determinado en la llamada (dinámico) | Capturado del scope léxico (estático) |
| `arguments` | Objeto propio creado en cada invocación | No propio — hereda del scope exterior |
| `new` (constructor) | Permitido — crea instancia | Prohibido — lanza `TypeError` |
| `super` | Disponible en métodos de clase | No tiene slot `[[HomeObject]]` propio |
| `prototype` | Propiedad `prototype` existe | No tiene propiedad `prototype` |
| `name` | Del identificador o declaración | Inferido del contexto (puede ser vacío) |
| Generadora | `function*` permitido | No existe `*(() => {})` |

## Cuándo usar cada una

Las arrows son la elección por defecto para callbacks funcionales, closures concisas y cualquier función que deba heredar el `this` del contexto circundante. Las funciones regulares son necesarias para métodos de objeto, constructores y funciones generadoras.

## Notas de la sección

- [[01 Sintaxis y Return Implícito]] — formas de escritura, return implícito, destructuring y rest en parámetros
- [[02 Sin this Propio]] — binding léxico de `this`, comparación con `call`/`apply`/`bind`, trampa del método de objeto
- [[03 Sin arguments ni new]] — ausencia de `arguments`, alternativa con rest params, por qué no pueden ser constructoras
- [[04 Cuándo Usarlas]] — tabla de decisión, recetas y antipatrones

## Notas relacionadas

- [[02 Funciones de Orden Superior/index|Funciones de Orden Superior]] — las arrows son el callback idiomático de las HOFs
- [[01 Conceptos Fundamentales/04 Composición de Funciones|Composición de Funciones]] — las arrows unarias encajan directamente en `pipe`/`compose`
