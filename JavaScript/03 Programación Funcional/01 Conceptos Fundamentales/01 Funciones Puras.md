---
title: Funciones Puras — Determinismo y ausencia de efectos
aliases:
  - funciones puras
  - pure functions
  - función pura
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
order: 1
---

# Funciones Puras

> [!definicion]
> Una **función pura** cumple dos condiciones de forma simultánea: (1) **determinismo** — dados los mismos argumentos, siempre retorna el mismo valor; (2) **ausencia de efectos secundarios observables** — no modifica nada fuera de su propio scope de entrada/salida. Si falta cualquiera de las dos condiciones, la función es impura.

```js
// Pura: mismo input → mismo output, nada externo
const sumar = (a, b) => a + b;
sumar(3, 4); // → 7, siempre

// Impura: depende de estado externo mutable
let base = 10;
const sumarABase = n => base + n;
sumarABase(5); // → 15 ahora, pero si base cambia el resultado cambia
```

## Qué hace pura a una función — y qué la rompe

| Condición | Pura | Impura |
|---|---|---|
| Mismo input → mismo output | siempre | no garantizado |
| Lee variables externas mutables | no | sí |
| Modifica argumentos | no | sí |
| Llama a I/O (console, red, DOM) | no | sí |
| Usa Math.random o Date.now | no | sí |
| Lanza excepciones | no | sí |

## Ejemplos puros en el lenguaje

`Math.max`, `Math.min`, `Math.abs`, `String.prototype.toUpperCase`, `String.prototype.slice`, `Array.prototype.map`, `Array.prototype.filter`, `Array.prototype.reduce` y todas las operaciones aritméticas son puras: reciben valores, retornan valores nuevos, no tocan nada externo.

```js
const duplicar = arr => arr.map(x => x * 2);
duplicar([1, 2, 3]); // → [2, 4, 6]
duplicar([1, 2, 3]); // → [2, 4, 6]  — idéntico, siempre
```

## Ejemplos impuros frecuentes

```js
// Math.random: no determinista
const tirarDado = () => Math.floor(Math.random() * 6) + 1;

// Date.now: depende del instante de ejecución
const timestamp = () => Date.now();

// console.log: efecto secundario de I/O
const logear = x => { console.log(x); return x; };

// Mutación de argumento: el caller queda afectado
const agregarItem = (arr, item) => { arr.push(item); return arr; };
```

## La regla de las mismas entradas

Una función que lee una variable global mutable **no es pura** aunque no produzca efectos visibles en el exterior — su output puede cambiar sin cambiar los argumentos declarados:

```js
// Impura aunque no haga nada "visible"
let multiplicador = 3;
const escalar = x => x * multiplicador;

escalar(5); // → 15
multiplicador = 10;
escalar(5); // → 50  — mismo argumento, distinto resultado
```

La corrección es inyectar la dependencia como parámetro:

```js
// Ahora sí es pura
const escalar = (x, factor) => x * factor;
escalar(5, 3);  // → 15
escalar(5, 10); // → 50  — predecible dado el input completo
```

## Recetas comunes

### Transformar una función impura en pura por inyección de dependencias

```js
// Impura: accede a Date directamente
const crearEvento = nombre => ({
  nombre,
  creadoEn: Date.now(), // no determinista
});

// Pura: el caller controla el timestamp
const crearEvento = (nombre, ahora) => ({
  nombre,
  creadoEn: ahora,
});

// En el borde impuro del sistema:
const evento = crearEvento("login", Date.now());
```

### Evitar mutación de argumentos

```js
// Impura
const ordenar = arr => { arr.sort(); return arr; };

// Pura: opera sobre una copia
const ordenar = arr => [...arr].sort();
```

### Funciones puras con objetos

```js
// Impura
const activar = usuario => { usuario.activo = true; return usuario; };

// Pura: produce un objeto nuevo
const activar = usuario => ({ ...usuario, activo: true });
```

## Idempotencia — concepto relacionado pero distinto

Una función **idempotente** produce el mismo resultado si se aplica una o múltiples veces: `f(f(x)) === f(x)`. No requiere ausencia de efectos secundarios — `setear(usuario, { activo: true })` puede ser idempotente aunque mute el objeto. La pureza implica determinismo; la idempotencia implica estabilidad ante repetición. Son propiedades ortogonales.

```js
// Idempotente pero impura (muta el objeto)
const activar = usuario => { usuario.activo = true; return usuario; };
activar(activar(u)); // mismo resultado que activar(u)

// Pura e idempotente
const clampPositivo = x => Math.max(0, x);
clampPositivo(clampPositivo(-5)); // → 0 === clampPositivo(-5)
```

## Por qué importan las funciones puras

Las funciones puras son **testeables sin mocks**: basta afirmar `expect(f(input)).toBe(output)` sin preparar estado global. Son **memoizables**: si el mismo input siempre da el mismo output, el resultado se puede cachear. Son **paralelizables**: sin estado compartido no hay condiciones de carrera. Son **predecibles**: leer la firma y el cuerpo es suficiente para entender el comportamiento completo.

## Cómo funciona por dentro

El motor JS no distingue funciones puras de impuras — ambas son objetos `Function` en el heap. La pureza es una propiedad **semántica** que el programador garantiza, no una que el runtime impone. Herramientas como `eslint-plugin-fp` o el modo `strict` de TypeScript pueden detectar lecturas de variables externas o mutaciones de argumentos, pero no son exhaustivas. La garantía real viene del diseño: pasar todas las dependencias como parámetros y no modificar nada que no se haya creado dentro de la función.

> [!tip] Buenas prácticas
> - Usar `const` para todos los bindings dentro de la función — señal de intención pura.
> - Pasar el tiempo (`Date.now()`) y los valores aleatorios como argumentos; llamarlos en el borde impuro.
> - En tests, si necesitas un mock para testear una función, es síntoma de que no es pura — refactorizar primero.
> - Preferir retornar un nuevo objeto/array antes que mutar el argumento.

> [!warning] Errores comunes
> - **Leer `this` en funciones que se pasan como callbacks**: el valor de `this` puede cambiar según el contexto de llamada, rompiendo el determinismo.
> - **Asumir que `Object.freeze` hace pura a la función**: `freeze` impide mutación del objeto desde fuera, pero si la función llama a `Math.random` sigue siendo impura.
> - **Mutación silenciosa de arrays**: `sort`, `reverse`, `splice`, `push`, `pop`, `shift`, `unshift` mutan el array original — hacer siempre `[...arr].sort()` antes.
> - **Creer que `try/catch` hace pura una función con lanzamiento de excepciones**: atrapar la excepción dentro no elimina el efecto — aún depende de errores externos.

## Notas relacionadas

- [[02 Inmutabilidad]] — técnica complementaria para garantizar la pureza al trabajar con objetos.
- [[03 Efectos Secundarios]] — qué son y cómo aislarlos cuando son inevitables.
- [[04 Composición de Funciones]] — las funciones puras son las piezas ideales para componer.
