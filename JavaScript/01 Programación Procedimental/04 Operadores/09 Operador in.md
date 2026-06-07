---
title: Operador in — Comprobar pertenencia de propiedades
aliases:
  - operador in js
  - in operator javascript
tags:
  - javascript
  - api/concepto
  - operadores
draft: false
---

# Operador in

> [!definicion]
> El operador `in` comprueba si una propiedad existe en un objeto **o en su cadena de prototipos**. Devuelve `true` si la propiedad (indicada como string, número o Symbol) existe en el objeto destino, `false` en caso contrario. A diferencia de `hasOwnProperty`, incluye propiedades heredadas.

```js
const persona = { nombre: "Ana", edad: 30 };

"nombre" in persona    // → true  (propiedad propia)
"toString" in persona  // → true  (heredada de Object.prototype)
"altura" in persona    // → false (no existe)
```

## Sintaxis

```js
"propiedad" in objeto
indice in array
Symbol.iterator in objeto
```

El operando izquierdo se convierte a string (o se usa como Symbol si es Symbol). El derecho debe ser un objeto; si es un primitivo lanza `TypeError`.

## in vs hasOwnProperty vs Object.hasOwn

| Método | Propias | Heredadas | Moderno |
|---|---|---|---|
| `"p" in obj` | sí | sí | — |
| `obj.hasOwnProperty("p")` | sí | no | no |
| `Object.hasOwn(obj, "p")` | sí | no | sí (ES2022) |

```js
const obj = { a: 1 };

"a" in obj                   // → true
"toString" in obj            // → true  (heredada)

obj.hasOwnProperty("a")      // → true
obj.hasOwnProperty("toString") // → false

Object.hasOwn(obj, "a")      // → true
Object.hasOwn(obj, "toString") // → false
```

> [!tip] Prefiere Object.hasOwn sobre hasOwnProperty
> `hasOwnProperty` puede ser sobrescrito en el propio objeto o no existir si el objeto fue creado con `Object.create(null)`. `Object.hasOwn(obj, prop)` es seguro en ambos casos y es la forma recomendada en código moderno.

## in con arrays

Con arrays, `in` comprueba si el **índice** existe (no si el valor existe):

```js
const arr = [10, 20, 30];
0 in arr    // → true   (índice 0 existe)
3 in arr    // → false  (índice 3 no existe)
10 in arr   // → false  (10 no es un índice válido, es un valor)

// Array con hueco (sparse array)
const sparse = [1, , 3];
1 in sparse   // → false  (hueco — el índice no tiene propiedad)
```

## in con Symbols

```js
const id = Symbol("id");
const obj = { [id]: 42 };

id in obj          // → true
"id" in obj        // → false (el Symbol y el string "id" son distintos)
Symbol.iterator in [] // → true (los arrays tienen Symbol.iterator)
```

## Uso como type narrowing

El operador `in` es la forma idiomática de comprobar si un objeto tiene una propiedad para distinguir entre tipos (patrón habitual en TypeScript pero útil en JS puro):

```js
function procesarEvento(evento) {
  if ("clientX" in evento) {
    // Es un MouseEvent
    console.log(evento.clientX, evento.clientY);
  } else if ("key" in evento) {
    // Es un KeyboardEvent
    console.log(evento.key);
  }
}
```

## Recetas comunes

### Comprobar soporte de API del navegador

```js
if ("geolocation" in navigator) {
  navigator.geolocation.getCurrentPosition(/* ... */);
}

if ("IntersectionObserver" in window) {
  // La API está disponible
}
```

### Comprobar propiedad opcional antes de acceder

```js
// Sin optional chaining
if ("config" in opciones && "timeout" in opciones.config) {
  usar(opciones.config.timeout);
}

// Con optional chaining (más moderno)
const timeout = opciones?.config?.timeout;
```

### Validar que un objeto tiene las propiedades esperadas

```js
function esRespuestaValida(obj) {
  return (
    typeof obj === "object" &&
    obj !== null &&
    "data" in obj &&
    "status" in obj
  );
}
```

## Cómo funciona por dentro

El motor recorre la **cadena de prototipos** del objeto de derecha buscando una propiedad con el nombre indicado. Internamente es el mismo algoritmo `[[HasProperty]]` que usa `for...in`. La búsqueda es O(profundidad de la cadena de prototipos), que en la práctica es siempre muy corta (1-4 niveles). Las propiedades no enumerables también son encontradas por `in` (a diferencia de `for...in` que solo itera enumerables).

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `in` cuando necesites incluir propiedades heredadas (comprobaciones de API, type narrowing).
> - Usa `Object.hasOwn(obj, prop)` cuando necesites verificar solo propiedades propias.
> - Para comprobar si un array **contiene un valor** (no un índice), usa `Array.prototype.includes()`.
> - Combina `typeof obj === "object" && obj !== null` antes de usar `in` si el objeto podría ser `null` o un primitivo.

## Errores comunes

> [!warning] Trampas
> - **`in` con el lado derecho como primitivo**: `"p" in "string"` lanza `TypeError`. El lado derecho siempre debe ser un objeto.
> - **Usar `in` en arrays para buscar valores**: `10 in [10, 20]` es `false` porque busca el índice `10`, no el valor `10`.
> - **Asumir que `in` solo busca propiedades propias**: incluye toda la cadena de prototipos.
> - **Olvidar `Object.hasOwn` para objetos sin prototipo**: `Object.create(null)` crea objetos sin `hasOwnProperty`; `Object.hasOwn` funciona con ellos.

## Notas relacionadas

- [[02 Variables/06 Ámbito (scope)/04 Lexical Scope]] — la cadena de prototipos y la cadena de ámbitos son análogos distintos.
- [[02 Programación Orientada a Objetos/02 Prototipos/01 Cadena de Prototipos]] — cómo funciona la cadena que `in` recorre.
- [[08 Optional Chaining (?.)]] — alternativa moderna para acceso seguro a propiedades anidadas.
- [[index]] — los operadores en contexto.
