---
title: SyntaxError — error de sintaxis inválida en el parseo
aliases:
  - SyntaxError
  - syntax error js
tags:
  - javascript
  - api/clase
  - errores
objeto: Error
tipo: clase
retorna: "-"
muta: false
asincrono: false
draft: false
---

# SyntaxError

> [!definicion]
> `SyntaxError` se lanza cuando el motor de JavaScript detecta código con sintaxis inválida **durante la fase de parseo**, antes de que se ejecute ninguna instrucción. El script completo se rechaza; ninguna línea del archivo corre.

```js
// Este fragmento no puede ejecutarse:
// function f() { return; }}}  → SyntaxError: Unexpected token '}'

// Lo que SÍ se puede capturar: JSON.parse con entrada inválida
try {
  JSON.parse('{ "clave": }');
} catch (err) {
  console.log(err instanceof SyntaxError); // → true
  console.log(err.message); // → "Unexpected token '}', ..."
}
```

**`name`:** `"SyntaxError"` — propiedad heredada de `Error.prototype` sobrescrita por el constructor.

## Cuándo se puede (y no se puede) capturar

`SyntaxError` lanzado durante el parseo del script principal **no es capturable con `try/catch`**: el bloque `try` nunca se ejecuta porque el script falla antes. La única forma de interceptarlo es desde el entorno host (browser, Node).

Las situaciones en que `try/catch` **sí captura** `SyntaxError`:

| Contexto | Capturable | Ejemplo |
|----------|-----------|---------|
| `JSON.parse()` con string inválido | Sí | `JSON.parse("{")` |
| `eval()` con código inválido | Sí | `eval("{{")` |
| `new Function(...)` con cuerpo inválido | Sí | `new Function("{{")` |
| Script principal con sintaxis rota | No | `function f( {` en el archivo |
| Módulo importado con sintaxis rota | No | `import "./roto.js"` |

## Causas comunes

| Causa | Ejemplo de código | Mensaje habitual |
|-------|-------------------|-----------------|
| Llave/paréntesis sin cerrar | `function f() {` | `Unexpected end of input` |
| `return` fuera de función | `return 5;` a nivel de módulo | `Illegal return statement` |
| `await` fuera de función async | `const x = await fetch(url);` en top-level sin módulo | `await is only valid in async functions` |
| `import` dentro de bloque | `if (x) { import './a.js' }` | `Cannot use import statement` |
| Label duplicado | `L: L: for(;;){}` | `Label 'L' has already been declared` |
| Destructuring inválido | `const [a b] = [1, 2]` | `Unexpected identifier 'b'` |

## Receta: capturar SyntaxError de JSON.parse

El caso de producción más frecuente es deserializar datos externos (respuestas de API, `localStorage`):

```js
function parseJSON(raw) {
  try {
    return { ok: true, data: JSON.parse(raw) };
  } catch (err) {
    if (err instanceof SyntaxError) {
      return { ok: false, error: err.message };
    }
    throw err; // otro tipo de error: no silenciar
  }
}

const result = parseJSON('{"nombre": "Ana"}');
// → { ok: true, data: { nombre: "Ana" } }

const bad = parseJSON("no es json");
// → { ok: false, error: "Unexpected token 'o', ..." }
```

> [!tip]
> Ante cualquier `JSON.parse` que reciba datos externos (localStorage, fetch, postMessage), envolver en `try/catch` es obligatorio. Los datos externos no están garantizados como JSON válido.

> [!warning]
> Un `SyntaxError` en el script principal no produce ninguna excepción capturada en ese mismo script. Si el módulo falla al parsear, `window.onerror` o el listener de `error` en el worker son el único punto de intercepción.

## Notas relacionadas

- [[01 Tipos de Errores/index | Tipos de Errores]]
- [[02 try - catch - finally/01 try y catch | try y catch]]
