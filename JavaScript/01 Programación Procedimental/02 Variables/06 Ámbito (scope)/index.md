---
title: Ámbito (scope) — Los cuatro tipos de ámbito en JavaScript
aliases:
  - scope
  - ámbito JS
  - alcance de variables
tags:
  - javascript
  - api/concepto
  - variables
tipo: concepto
draft: false
order: 1
---

# Ámbito (scope)

> [!definicion]
> El **ámbito** (*scope*) es el conjunto de reglas que determina desde qué partes del código un identificador es visible y accesible. JavaScript usa **ámbito léxico estático**: el ámbito de una variable queda fijado por su posición en el código fuente, no por el flujo de ejecución en tiempo de ejecución.

```js
const global = "visible en todas partes";

function exterior() {
  const enFuncion = "visible en exterior y en interior";

  function interior() {
    const enBloque = "visible solo en interior";
    console.log(global);    // ✓
    console.log(enFuncion); // ✓
    console.log(enBloque);  // ✓
  }

  console.log(enBloque); // ReferenceError
}
```

## Los cuatro tipos de ámbito

### Global scope

Las variables declaradas fuera de cualquier función o bloque pertenecen al ámbito global y son accesibles desde cualquier punto del programa. En el navegador, `var` en el global se convierte en propiedad de `window`; `let`/`const` no. Ver [[01 Global Scope]].

### Function scope

Cada llamada a una función crea su propio ámbito. Las variables declaradas dentro —incluidos los parámetros— no son accesibles desde el exterior. `var` respeta este ámbito aunque no respeta el ámbito de bloque. Ver [[02 Function Scope]].

### Block scope

Cualquier par de llaves `{}` crea un ámbito de bloque para variables `let` y `const`. Bloques `if`, `for`, `while`, `try/catch` y bloques desnudos generan este ámbito. `var` ignora el block scope. Ver [[03 Block Scope]].

### Lexical scope

El ámbito se determina por dónde está **escrita** la función, no desde dónde se llama. Una función anidada tiene acceso al ámbito de todas sus funciones contenedoras. Este mecanismo es la base de los closures. Ver [[04 Lexical Scope]].

## La cadena de ámbitos (scope chain)

Cuando el motor no encuentra un identificador en el ámbito local, lo busca en el ámbito exterior inmediato, y así sucesivamente hasta el global. Si no lo encuentra en ningún nivel, lanza `ReferenceError`.

```js
const nivel1 = "global";

function a() {
  const nivel2 = "función a";

  function b() {
    const nivel3 = "función b";

    function c() {
      // scope chain: c → b → a → global
      console.log(nivel3); // "función b"
      console.log(nivel2); // "función a"
      console.log(nivel1); // "global"
    }
    c();
  }
  b();
}
a();
```

La búsqueda sube por la cadena pero **nunca baja**: el ámbito exterior no puede acceder a variables definidas en ámbitos interiores.

## Tabla comparativa de tipos de ámbito

| Tipo | Creado por | Declaraciones que lo respetan | Acceso desde exterior |
|---|---|---|---|
| Global | nivel raíz del script/módulo | todas | sí (es el más externo) |
| Función | llamada a función | `var`, `let`, `const` | no |
| Bloque | cualquier `{}` | `let`, `const` | no |
| Léxico | posición en el código fuente | todas | funciones internas sí; externas no |

## Notas relacionadas

- [[01 Global Scope]]
- [[02 Function Scope]]
- [[03 Block Scope]]
- [[04 Lexical Scope]]
- [[../04 Hoisting|Hoisting]]
- [[../05 Temporal Dead Zone|Temporal Dead Zone]]
