---
title: Nullish Coalescing (??) — valor por defecto para null/undefined
aliases:
  - nullish coalescing
  - operador ??
  - doble interrogación JS
tags:
  - javascript
  - api/operador
  - operadores
objeto: global
tipo: operador
retorna: el operando izquierdo si no es nullish, el derecho si lo es
muta: false
asincrono: false
draft: false
order: 7
---

# Nullish Coalescing `??`

> [!definicion] Nullish Coalescing
> `a ?? b` devuelve `a` si `a` no es `null` ni `undefined`; de lo contrario devuelve `b`. Solo dos valores son "nullish": `null` y `undefined`. Cualquier otro valor falsy (`0`, `""`, `false`, `NaN`) se considera presente y no activa el fallback.

```js
null      ?? "default"  // → "default"
undefined ?? "default"  // → "default"
0         ?? "default"  // → 0       (0 no es nullish)
""        ?? "default"  // → ""      (string vacío no es nullish)
false     ?? "default"  // → false   (false no es nullish)
NaN       ?? "default"  // → NaN     (NaN no es nullish)
```

## Diferencia clave con `||`

`||` devuelve el fallback para cualquier valor falsy. `??` solo lo hace para `null` y `undefined`. Esta distinción es crítica cuando `0`, `""` o `false` son valores válidos en el dominio.

```js
// Configuración donde 0 y "" son valores legítimos
const config = { timeout: 0, label: "", retries: null };

// || sobreescribe valores válidos
config.timeout  || 3000   // → 3000  (INCORRECTO: 0 es un timeout válido)
config.label    || "?"    // → "?"   (INCORRECTO: "" puede ser "sin etiqueta")
config.retries  || 3      // → 3     (correcto: null sí es ausente)

// ?? respeta valores falsy no nullish
config.timeout  ?? 3000   // → 0     (CORRECTO)
config.label    ?? "?"    // → ""    (CORRECTO)
config.retries  ?? 3      // → 3     (correcto: null es nullish)
```

## Cortocircuito

`??` cortocircuita: si el operando izquierdo no es nullish, el derecho no se evalúa.

```js
let sideEffect = false;
function expensive() { sideEffect = true; return "valor"; }

const result = "existente" ?? expensive();
// → "existente"; expensive() nunca se llama
console.log(sideEffect); // → false
```

## `??=` — Nullish Assignment (ES2021)

Asigna el valor de la derecha solo si la variable es nullish. Equivale a `a ?? (a = b)`.

```js
let user = { name: "Ana", age: null, score: 0 };

user.age   ??= 18;  // age era null → se asigna 18
user.score ??= 100; // score era 0 (no nullish) → NO se asigna
user.name  ??= "Desconocido"; // name era "Ana" → NO se asigna

console.log(user); // → { name: "Ana", age: 18, score: 0 }

// Inicializar propiedades opcionales de un objeto de configuración
function configure(opts = {}) {
  opts.host    ??= "localhost";
  opts.port    ??= 3000;
  opts.timeout ??= 0;  // 0 es un timeout válido
  return opts;
}
configure({ port: 8080 }); // → { host: "localhost", port: 8080, timeout: 0 }
```

## Combinación con `?.` — el patrón canónico

`??` y `?.` se complementan: `?.` devuelve `undefined` cuando la cadena de acceso falla, y `??` convierte ese `undefined` en un valor por defecto.

```js
const user = {
  profile: {
    settings: null
  }
};

// Acceso seguro con fallback
const theme  = user?.profile?.settings?.theme ?? "light";
const volume = user?.profile?.settings?.volume ?? 50;
// → "light", 50  (aunque settings sea null)

// Sin ??, habría que escribir:
const theme2 = (user && user.profile && user.profile.settings && user.profile.settings.theme) || "light";
// (además, || reemplaza valores falsy, no solo null/undefined)
```

## Restricción de mezcla con `&&` y `||`

`??` no puede mezclarse directamente con `&&` o `||` sin paréntesis. La especificación lo prohíbe para evitar ambigüedad de precedencia: produce `SyntaxError`.

```js
// SyntaxError: hay que añadir paréntesis explícitos
null || undefined ?? "default"  // SyntaxError
null && undefined ?? "default"  // SyntaxError

// Con paréntesis: correcto
(null || undefined) ?? "default"   // → "default"
null || (undefined ?? "default")   // → "default"
(null && "a") ?? "default"         // → "default"
```

## Recetas comunes

```js
// Valor por defecto de parámetro (cuando 0 o "" son válidos)
function resize(width, height) {
  width  = width  ?? 800;
  height = height ?? 600;
  return { width, height };
}
resize(0, 0)     // → { width: 0, height: 0 }  (correcto con ??)
resize(0, 0, /* con || */); // daría { width: 800, height: 600 } — incorrecto

// Acceso a propiedad anidada con fallback
const title = response?.data?.article?.title ?? "Sin título";

// Inicialización perezosa de caché
cache[key] ??= computeExpensiveValue(key);

// Merge con valores por defecto
const merged = { ...defaults, ...userConfig };
// Si el usuario pasó 0, se respeta; ?? dentro de reduce para mayor control:
const getOpt = (key) => userConfig[key] ?? defaults[key];
```

## Cómo funciona por dentro

`??` implementa el algoritmo Nullish Coalescing Evaluation de la spec (ES2020): evalúa el operando izquierdo; si el resultado es `null` o `undefined` (comprobación de identidad, sin coerción), evalúa y devuelve el operando derecho; de lo contrario devuelve el operando izquierdo tal cual. La prohibición de mezclar con `&&`/`||` está codificada directamente en la gramática del parser (Early Error), no es un error de ejecución.

> [!tip] Buenas prácticas
> - Usar `??` para valores por defecto cuando `0`, `""`, `false` o `NaN` son valores del dominio.
> - Combinar con `?.` para acceso seguro a objetos con estructura variable.
> - Preferir `??=` sobre `if (x === null || x === undefined) x = defaultValue` para inicialización de propiedades.
> - Cuando el valor solo puede ser `null`/`undefined` o un valor concreto (sin otros falsy posibles), `??` y `||` son equivalentes, pero `??` comunica la intención más claramente.

> [!warning] Errores comunes
> **Mezclar `??` con `||` o `&&` sin paréntesis:** SyntaxError en tiempo de parseo. Siempre usar paréntesis explícitos.
>
> **Usar `||` cuando `0` o `""` son valores válidos:** el bug más común al migrar código pre-ES2020. Revisar cada `||` usado como valor por defecto y evaluar si debería ser `??`.
>
> **`??` no comprueba falsiness, sino nullishness:** `NaN ?? "fallback"` devuelve `NaN`, no `"fallback"`. Si NaN debe activar el fallback, comprobar con `Number.isNaN` antes.

## Notas relacionadas

- [[04 Lógicos]] — `||` como alternativa y diferencias de comportamiento
- [[08 Optional Chaining (?.)]] — `?.` devuelve `undefined` cuando falla; `??` convierte ese `undefined` en fallback
- [[01 Asignación]] — `??=` (nullish assignment, ES2021)
- [[03 Comparación]] — por qué `null == undefined` es `true` y ambos son "nullish"
