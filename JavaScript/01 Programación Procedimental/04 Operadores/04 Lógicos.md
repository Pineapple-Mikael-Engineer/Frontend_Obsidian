---
title: Operadores Lógicos — &&, ||, !
aliases:
  - operadores lógicos JS
  - logical operators
  - short-circuit
  - AND OR NOT JS
tags:
  - javascript
  - api/operador
  - operadores
objeto: global
tipo: operador
retorna: uno de los operandos (no necesariamente boolean)
muta: false
asincrono: false
draft: false
---

# Operadores Lógicos

> [!definicion] Operadores lógicos
> `&&`, `||` y `!` evalúan la veracidad de sus operandos. La clave de JS: `&&` y `||` **no devuelven `true`/`false`** — devuelven uno de sus operandos tal cual. Esto habilita patrones de cortocircuito que van más allá de la lógica booleana pura.

```js
// Devuelven el operando, no un boolean
"hola" && 42     // → 42        (primero truthy → devuelve el segundo)
null && 42       // → null      (primero falsy → devuelve el primero)
false || "def"   // → "def"     (primero falsy → devuelve el segundo)
"ok"  || "def"   // → "ok"      (primero truthy → devuelve el primero)
!0               // → true      (! sí convierte a boolean)
```

## Truthy y Falsy

`&&` y `||` evalúan la "veracidad" de cada operando usando el algoritmo ToBoolean de la spec. Los únicos valores **falsy** en JS son:

```js
false, 0, -0, 0n, "", null, undefined, NaN
```

Todo lo demás es truthy: `{}`, `[]`, `"0"`, `"false"`, `-1`, `Infinity`.

## `&&` — AND lógico

Devuelve el **primer valor falsy** encontrado, o el **último operando** si todos son truthy. Cortocircuita: si el primer operando es falsy, el segundo no se evalúa.

```js
// Devuelve primer falsy o el último
1 && 2 && 3        // → 3      (todos truthy → devuelve el último)
1 && null && 3     // → null   (null es falsy → para ahí)
0 && alert("no")   // → 0     (alert nunca se ejecuta)

// Guard pattern: ejecutar algo solo si existe
const user = getUser();
user && processUser(user);   // processUser solo se llama si user es truthy

// Acceso condicional (antes de optional chaining)
const name = user && user.profile && user.profile.name;
```

## `||` — OR lógico

Devuelve el **primer valor truthy** encontrado, o el **último operando** si todos son falsy. Cortocircuita: si el primer operando es truthy, el segundo no se evalúa.

```js
// Devuelve primer truthy o el último
1 || 2            // → 1      (1 es truthy → para ahí)
null || undefined || "fallback" // → "fallback"
false || 0 || ""  // → ""     (todos falsy → devuelve el último)

// Valor por defecto (patrón clásico)
function greet(name) {
  name = name || "Visitante";  // si name es falsy, usa "Visitante"
  return `Hola, ${name}`;
}
greet("Ana")  // → "Hola, Ana"
greet("")     // → "Hola, Visitante"  (cuidado: "" es falsy)
greet(0)      // → "Hola, Visitante"  (cuidado: 0 es falsy)
```

## `!` — NOT lógico

Convierte el operando a boolean (ToBoolean) y lo invierte. `!!` se usa como cast explícito a boolean.

```js
!true     // → false
!false    // → true
!0        // → true
!""       // → true
!null     // → true
!{}       // → false  ({} es truthy)

// !! cast a boolean
!!0         // → false
!!"texto"   // → true
!!null      // → false
!![]        // → true   (array vacío es truthy)

// Equivalente más legible
Boolean(0)  // → false
```

## Diferencia entre `||` y `??`

`||` trata todos los valores falsy como "ausentes". `??` (nullish coalescing) solo trata `null` y `undefined` como ausentes. Crucial cuando `0`, `""` o `false` son valores legítimos.

```js
const config = { timeout: 0, label: "" };

// || sobreescribe valores válidos falsy
config.timeout || 3000   // → 3000  (MALO: 0 es un timeout válido)
config.label   || "sin nombre" // → "sin nombre" (MALO: "" puede ser válido)

// ?? respeta valores falsy no nullish
config.timeout ?? 3000   // → 0     (CORRECTO: 0 no es nullish)
config.label   ?? "sin nombre" // → ""  (CORRECTO: "" no es nullish)
```

## Short-circuit: efectos secundarios y evaluación perezosa

El cortocircuito no es solo optimización: determina si los efectos secundarios del segundo operando ocurren.

```js
// El segundo operando puede no ejecutarse nunca
let logged = false;
function logAndReturn(val) { logged = true; return val; }

true && logAndReturn(42);  // → 42,  logged = true
false && logAndReturn(42); // → false, logged sigue false (no se llamó)

// Patrón: ejecutar función solo si condición se cumple
isAdmin && deleteRecord(id);  // deleteRecord no se llama si !isAdmin

// Patrón: render condicional en JSX
const el = isVisible && <Component />;
```

## Recetas comunes

```js
// Guard: hacer algo solo si un valor existe
obj && obj.method();          // antes de ?.
obj?.method();                // forma moderna con optional chaining

// Valor por defecto nullish-safe
const port = options.port ?? 3000;

// Valor por defecto con fallback chain
const val = a || b || c || "default";

// Cast a boolean
const hasItems = !!arr.length;

// Toggle boolean
let active = true;
active = !active;  // → false

// Acceso seguro con fallback
const title = page?.meta?.title ?? "Sin título";
```

## Cómo funciona por dentro

`&&` y `||` siguen el algoritmo de Short-Circuit Evaluation de la spec: evalúan el operando izquierdo, obtienen su valor abstracto booleano con ToBoolean, y según el resultado deciden si evaluar el derecho. El valor **devuelto** es siempre uno de los operandos originales (sin conversión a boolean), lo que distingue a JS de lenguajes como Python (`and`/`or` también devuelven el operando) pero difiere de C o Java (que siempre devuelven `bool`).

`!` sí aplica ToBoolean y devuelve el resultado invertido como boolean primitivo.

> [!tip] Buenas prácticas
> - Usar `??` en lugar de `||` para valores por defecto cuando `0`, `""` o `false` son válidos.
> - Preferir `?.` sobre el patrón `obj && obj.prop` para acceso seguro.
> - `!!x` es idiomático en JS pero `Boolean(x)` comunica la intención más claramente en código de equipo.
> - Evitar expresiones de cortocircuito muy largas (`a && b && c && d`); considerar una variable intermedia o una función.

> [!warning] Errores comunes
> **`||` con número `0` o string vacío:** `count || 10` reemplaza `0` con `10`. Si `0` es un valor válido, usar `count ?? 10`.
>
> **Confundir `&&` en condición con `&&` como guard:** `if (a && b)` evalúa el booleano del resultado; como expresión, `a && b` puede devolver `a` (un valor truthy/falsy), no necesariamente `true`.
>
> **Cadenas de `||` que enmascan errores:** `config.url || env.URL || "http://localhost"` puede silenciar un `undefined` accidental. Mejor validar explícitamente.

## Notas relacionadas

- [[07 Nullish Coalescing (??)]] — `??` como alternativa a `||` para valores nullish
- [[08 Optional Chaining (?.)]] — reemplaza el patrón `obj && obj.prop`
- [[01 Asignación]] — `&&=`, `||=`, `??=` (logical assignment, ES2021)
- [[03 Comparación]] — los booleanos que alimentan las condiciones
