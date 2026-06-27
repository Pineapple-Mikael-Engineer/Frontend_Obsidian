---
title: Acceso a Propiedades — notación dot, bracket y optional chaining
aliases:
  - dot notation
  - bracket notation
  - optional chaining
  - acceso a propiedades
tags:
  - javascript
  - api/concepto
  - objetos
objeto: Object
tipo: concepto
draft: false
order: 3
---

# Acceso a Propiedades (dot y bracket)

> [!definicion]
> El acceso a propiedades de un objeto se realiza mediante dos notaciones equivalentes en mecanismo pero distintas en capacidad: **dot** (`obj.prop`) y **bracket** (`obj["prop"]`). Ambas invocan el mismo algoritmo interno `[[Get]]`. La diferencia es cómo se resuelve la clave: literal de identificador en dot, evaluación de expresión en bracket.

```js
const usuario = { nombre: "Ana", "correo electronico": "ana@mail.com" };

console.log(usuario.nombre);              // → "Ana"      (dot)
console.log(usuario["nombre"]);           // → "Ana"      (bracket, equivalente)
console.log(usuario["correo electronico"]); // → "ana@mail.com" (solo posible con bracket)
```

## Notación dot

La clave debe ser un **identificador JavaScript válido**: empieza con letra, `_` o `$`; no contiene espacios ni caracteres especiales; no es una palabra reservada (aunque en la práctica `obj.class` funciona en acceso de propiedad).

```js
const config = { maxRetries: 3, host: "localhost" };

console.log(config.maxRetries); // → 3
console.log(config.host);       // → "localhost"
```

## Notación bracket

Acepta cualquier expresión que produzca un string (o Symbol). La expresión se evalúa en tiempo de ejecución.

```js
const campo = "maxRetries";
console.log(config[campo]);             // → 3    (clave dinámica desde variable)
console.log(config["max" + "Retries"]); // → 3    (clave construida en tiempo de ejecución)

// Clave numérica (convertida a string internamente)
const arr = [10, 20, 30];
console.log(arr[1]);   // → 20
console.log(arr["1"]); // → 20  (mismo resultado: "1" === String(1))
```

### Cuándo usar bracket obligatoriamente

- Claves con espacios, guiones u otros caracteres no válidos como identificador.
- Claves almacenadas en variables o calculadas dinámicamente.
- Claves que son Symbols.
- Acceso a índices de array.

## Acceso a propiedad inexistente

Si la propiedad no existe ni en el objeto ni en su cadena de prototipos, `[[Get]]` devuelve `undefined`. No se lanza ningún error. El error `TypeError: Cannot read properties of undefined` ocurre cuando se intenta acceder a una propiedad de `undefined` o `null`:

```js
const obj = { a: { b: 1 } };

console.log(obj.a.b);   // → 1
console.log(obj.x);     // → undefined (sin error)
console.log(obj.x.y);   // → TypeError: Cannot read properties of undefined (reading 'y')
```

## Optional chaining `?.`

El operador `?.` cortocircuita y devuelve `undefined` en lugar de lanzar si el receptor es `null` o `undefined`. Disponible desde ES2020.

```js
const datos = { usuario: null };

console.log(datos.usuario?.nombre);           // → undefined (sin TypeError)
console.log(datos.usuario?.direccion?.ciudad); // → undefined (encadenado profundo)
console.log(datos.usuario?.saludar());         // → undefined (también en llamadas a método)
console.log(datos.lista?.[0]);                 // → undefined (también con bracket)
```

El operador `?.` no silencia todos los errores: solo cortocircuita cuando el receptor inmediato es `null`/`undefined`. Si `datos` fuera `undefined`, `datos?.usuario` devolvería `undefined`; pero si `datos.usuario` devuelve un string, `datos.usuario?.nombre` simplemente devuelve `undefined` porque un string no tiene propiedad `nombre`.

## Escritura y el mecanismo `[[Set]]`

La asignación `obj.prop = valor` invoca `[[Set]]`, que comprueba el descriptor de la propiedad. Si es `writable: false` o tiene `set` definido, el comportamiento depende de esos atributos. El acceso de lectura (`[[Get]]`) y escritura (`[[Set]]`) son algoritmos distintos aunque usan la misma notación.

```js
const obj = {};
Object.defineProperty(obj, "x", { value: 1, writable: false });
obj.x = 99; // silencioso en modo no estricto; TypeError en "use strict"
console.log(obj.x); // → 1
```

## Cómo funciona por dentro

Dot y bracket compilan al mismo bytecode de acceso de propiedad. La diferencia es solo sintáctica: el parser transforma `obj.prop` en una llamada `[[Get]](obj, "prop")` y `obj[expr]` en `[[Get]](obj, ToString(expr))` (o mantiene el Symbol si `expr` es un Symbol). El algoritmo `[[Get]]` busca primero en las propiedades propias del objeto (consultando el descriptor), y si no la encuentra sube por `[[Prototype]]` hasta llegar a `null`.

> [!tip] Buenas prácticas
> - Usar dot para la mayoría de accesos: más legible y ligeramente más rápido de parsear.
> - Usar bracket cuando la clave es dinámica o contiene caracteres especiales.
> - Usar `?.` en cadenas de acceso donde algún nodo intermedio puede ser `null`/`undefined` (datos de API, config opcional).

> [!warning] Errores comunes
> - `obj.prop` cuando `prop` es una variable: accede a la clave literal `"prop"`, no al valor de la variable. Para claves dinámicas, usar `obj[prop]`.
> - Asumir que `?.` protege contra propiedades que no son `null`/`undefined` pero tampoco existen: devuelve `undefined` igualmente.
> - `delete obj?.prop` elimina la propiedad si el objeto existe; si el objeto es `null`/`undefined`, retorna `true` sin hacer nada.

## Notas relacionadas

- [[04 Propiedades Calculadas]] — claves dinámicas en creación de objeto
- [[08 Comprobar y Eliminar (in, hasOwnProperty, delete)]] — comprobar existencia antes de acceder
- [[01 Operadores/08 Optional Chaining (?.).md | Optional Chaining (?.)]] — detalle completo del operador
- [[02 Prototipos/index | Prototipos]] — cómo `[[Get]]` recorre la cadena de prototipos
