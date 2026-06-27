---
title: instanceof — Verificación de cadena de prototipos
aliases:
  - instanceof JS
  - operador instanceof
tags:
  - javascript
  - api/operador
  - tipos
objeto: global
tipo: operador
retorna: boolean
muta: false
asincrono: false
draft: false
order: 2
---

# instanceof

> [!definicion]
> `instanceof` es un operador binario que verifica si el `prototype` de una función constructora (o clase) aparece en la **cadena de prototipos** (`[[Prototype]]`) del objeto izquierdo. Retorna `true` si lo encuentra en cualquier nivel de la cadena; `false` si llega a `null` sin encontrarlo.

```js
[] instanceof Array       // → true
[] instanceof Object      // → true  (Array.prototype hereda de Object.prototype)
{} instanceof Object      // → true
"hola" instanceof String  // → false  (primitivos no tienen cadena de prototipos)
```

## Mecanismo: recorrido de `[[Prototype]]`

`objeto instanceof Constructor` ejecuta el siguiente algoritmo:

1. Obtiene `Constructor.prototype`.
2. Recorre la cadena `[[Prototype]]` del objeto: `objeto.__proto__`, `objeto.__proto__.__proto__`, etc.
3. Si en algún eslabón el valor es `=== Constructor.prototype`, retorna `true`.
4. Si llega a `null` (fin de la cadena), retorna `false`.

```js
const arr = [1, 2, 3];

// La cadena de arr:
// arr → Array.prototype → Object.prototype → null

arr instanceof Array    // → true  (Array.prototype encontrado)
arr instanceof Object   // → true  (Object.prototype encontrado)
arr instanceof Map      // → false (Map.prototype no está en la cadena)
```

## Con clases

`instanceof` funciona igualmente con clases ES6, ya que las clases son azúcar sintáctica sobre el sistema de prototipos:

```js
class Animal {
  constructor(nombre) { this.nombre = nombre; }
}

class Perro extends Animal {}

const rex = new Perro("Rex");

rex instanceof Perro    // → true
rex instanceof Animal   // → true  (herencia: Animal.prototype en la cadena)
rex instanceof Object   // → true
rex instanceof Array    // → false
```

## Primitivos: siempre `false`

Los primitivos no tienen cadena de prototipos como objetos. `instanceof` siempre retorna `false` para ellos:

```js
"hola" instanceof String    // → false  (primitivo, no objeto String)
42 instanceof Number         // → false
true instanceof Boolean      // → false

// Solo los wrappers (raramente usados) retornan true:
new String("hola") instanceof String   // → true
```

## Limitación: cross-realm

Cada contexto de ejecución (ventana de navegador, iframe, worker, módulo VM en Node.js) tiene su propio objeto `Array`, `Object`, etc. Un array creado en un iframe tiene `Array.prototype` del iframe, no el de la ventana padre, por lo que `instanceof Array` falla:

```js
// En un entorno con iframes:
const arrDeIframe = iframe.contentWindow.Array.from([1, 2, 3]);
arrDeIframe instanceof Array   // → false  (Array distinto)
Array.isArray(arrDeIframe)     // → true   (Array.isArray es inmune a esto)
```

Para arrays, la solución canónica es `Array.isArray()`. Para otros tipos, `Object.prototype.toString.call()`.

## `Symbol.hasInstance`: personalizar `instanceof`

La propiedad estática `Symbol.hasInstance` en una clase permite sobreescribir el comportamiento de `instanceof`:

```js
class EsNumero {
  static [Symbol.hasInstance](valor) {
    return typeof valor === "number";
  }
}

42 instanceof EsNumero     // → true
"hola" instanceof EsNumero // → false
NaN instanceof EsNumero    // → true  (typeof NaN === "number")
```

## Alternativas más robustas

**`Object.prototype.toString.call(valor)`** retorna el tag interno del tipo, inmune a cross-realm y útil para tipos built-in:

```js
Object.prototype.toString.call([])          // → "[object Array]"
Object.prototype.toString.call(null)        // → "[object Null]"
Object.prototype.toString.call(undefined)   // → "[object Undefined]"
Object.prototype.toString.call(new Date())  // → "[object Date]"
Object.prototype.toString.call(/re/)        // → "[object RegExp]"
Object.prototype.toString.call(42n)         // → "[object BigInt]"
```

**`Array.isArray(valor)`** — la forma idiomática para detectar arrays:

```js
Array.isArray([])      // → true
Array.isArray({})      // → false
Array.isArray("hola")  // → false
```

## Tabla comparativa: `typeof` vs `instanceof`

| Herramienta | Uso adecuado | No sirve para |
|---|---|---|
| `typeof` | Primitivos, distinguir `"function"` de `"object"` | Distinguir tipos de objetos entre sí |
| `instanceof` | Verificar herencia de clase/constructor | Primitivos, cross-realm, `null` |
| `Array.isArray()` | Detectar arrays (incluso cross-realm) | Otros tipos |
| `Object.prototype.toString` | Tipo interno preciso de cualquier valor | Clases custom sin `toStringTag` |

> [!tip] Buenas prácticas
> - Para arrays, usar `Array.isArray()` en lugar de `instanceof Array`.
> - Para detección de tipo en librerías que manejan múltiples realms, preferir `Object.prototype.toString.call()`.
> - `instanceof` es adecuado para jerarquías de clases propias donde el cross-realm no aplica.

> [!warning] Errores comunes
> - `"hola" instanceof String` → `false`: los primitivos string no son instancias de `String`.
> - `instanceof` falla silenciosamente en cross-realm: retorna `false` sin lanzar error.
> - `null instanceof Object` → `false` (a pesar de que `typeof null === "object"`).
> - Si el `prototype` del constructor se reemplaza tras crear la instancia, `instanceof` puede dar `false` inesperadamente.

## Notas relacionadas

- [[02 typeof|typeof]] — alternativa para primitivos e introspección básica
- [[06 symbol|symbol]] — `Symbol.hasInstance` para personalizar `instanceof`
- [[03 Tipos de Datos/index|Tipos de Datos]]
