---
title: "Operador Ternario — condición ? valorTrue : valorFalse"
aliases:
  - operador ternario JS
  - ternary operator
  - conditional expression
tags:
  - javascript
  - api/operador
  - operadores
objeto: global
tipo: operador
retorna: uno de los dos valores según la condición
muta: false
asincrono: false
draft: false
order: 6
---

# Operador Ternario

> [!definicion] Operador ternario
> `condición ? valorSiTrue : valorSiFalse` es el único operador ternario (tres operandos) de JS. A diferencia de `if/else`, es una **expresión**: produce un valor y puede aparecer en cualquier lugar donde se espera un valor. La condición se evalúa con ToBoolean.

```js
const edad = 20;
const estado = edad >= 18 ? "adulto" : "menor";
// → "adulto"

// Como expresión, puede usarse en una asignación, un argumento, una interpolación
const msg = `El usuario es ${edad >= 18 ? "adulto" : "menor"}.`;
console.log(msg); // → "El usuario es adulto."
```

## Expresión vs declaración

`if/else` es una declaración (statement): no produce valor y no puede aparecer dentro de otra expresión. El ternario es una expresión: produce un valor y puede estar en asignaciones, argumentos de función, JSX, templates literales, etc.

```js
// if/else NO puede usarse como valor
const x = if (a > b) a else b;  // SyntaxError

// Ternario sí puede
const x = a > b ? a : b;  // ✓ — máximo de dos valores

// En argumento de función
console.log(isLogged ? "Bienvenido" : "Inicia sesión");

// En JSX (React)
const el = <div>{isLoading ? <Spinner /> : <Content />}</div>;
```

## Anidamiento: cuándo evitarlo

El ternario puede anidarse, pero cada nivel adicional daña la legibilidad. Más de dos ramas → usar `if/else if` o un objeto de mapeo.

```js
// Anidamiento — difícil de leer
const nota = score >= 90 ? "A"
           : score >= 80 ? "B"
           : score >= 70 ? "C"
           : score >= 60 ? "D"
           : "F";

// Alternativa más legible: objeto de mapeo o función
function calcNota(score) {
  if (score >= 90) return "A";
  if (score >= 80) return "B";
  if (score >= 70) return "C";
  if (score >= 60) return "D";
  return "F";
}

// Alternativa: tabla
const NOTAS = [[90, "A"], [80, "B"], [70, "C"], [60, "D"]];
const nota = NOTAS.find(([min]) => score >= min)?.[1] ?? "F";
```

## Regla: valores vs acciones

El ternario es para **elegir un valor**. Si las dos ramas ejecutan efectos secundarios (llamadas a funciones con side effects, modificaciones de estado), prefiere `if/else`: la intención queda más clara y el código es más fácil de depurar.

```js
// BIEN: elegir un valor
const endpoint = isProduction ? API_URL : DEV_URL;
const className = isActive ? "btn-primary" : "btn-secondary";
const plural = count === 1 ? "item" : "items";

// EVITAR: efectos secundarios en ternario (usa if/else en su lugar)
isAdmin ? deleteRecord(id) : logUnauthorized(id);  // confuso

// MEJOR así:
if (isAdmin) {
  deleteRecord(id);
} else {
  logUnauthorized(id);
}
```

## Recetas comunes

```js
// Valor por defecto simple
const name = inputName ? inputName : "Anónimo";
// Preferir ?? si solo se quiere evitar null/undefined:
const name = inputName ?? "Anónimo";

// Clase CSS condicional
const cls = `btn ${isDisabled ? "btn--disabled" : "btn--active"}`;

// Renderizado condicional conciso (React/JSX)
{hasError && <ErrorMessage />}  // solo si truthy (sin else)
{hasError ? <ErrorMessage /> : <SuccessMessage />}  // con else

// Argumento condicional
fetchData(url, isAdmin ? { credentials: "include" } : {});

// Ternario con funciones que devuelven valor
const sorted = isDescending ? arr.toReversed() : arr;
```

## Cómo funciona por dentro

`? :` tiene asociatividad derecha y precedencia 3 (por encima de `=`, por debajo de `||`). El motor evalúa el primer operando, aplica ToBoolean, y solo evalúa el segundo **o** el tercer operando (nunca ambos): hay cortocircuito. El valor producido es el operando evaluado, sin conversión de tipo.

```js
// Asociatividad derecha: el ternario anidado a la derecha del :
// a ? b : c ? d : e  →  a ? b : (c ? d : e)
true ? "a" : false ? "b" : "c"   // → "a"
false ? "a" : false ? "b" : "c"  // → "c"

// Cortocircuito: solo una rama se evalúa
let n = 0;
true ? n++ : n--;
console.log(n); // → 1  (solo se evaluó n++)
```

> [!tip] Buenas prácticas
> - Ternario para una sola decisión binaria que produce un valor; `if/else` para acciones o más de dos ramas.
> - Si el ternario no cabe cómodamente en una línea, es señal de que la lógica es demasiado compleja para este operador.
> - En JSX, el ternario es el patrón estándar para renderizado condicional con ambas ramas; `&&` para renderizado condicional sin rama falsa.

> [!warning] Errores comunes
> **Ternario anidado ilegible:** más de dos niveles produce código que el lector debe "simular" mentalmente. Extraer a función o usar `if/else`.
>
> **Efectos secundarios ocultos:** si ambas ramas modifican estado, un lector puede no darse cuenta de que solo una se ejecuta. Usar `if/else` para hacer explícito el flujo.
>
> **Precedencia con `=`:** `a = b ? c : d` asigna el resultado del ternario a `a` (correcto). Pero `a ? b : c = d` asigna `d` a `c` si la condición es falsa (asociatividad `=` < ternario). Usar paréntesis para evitar ambigüedad.

## Notas relacionadas

- [[04 Lógicos]] — `&&` como alternativa cuando no hay rama falsa
- [[07 Nullish Coalescing (??)]] — alternativa al ternario para valores por defecto nullish
- [[01 Asignación]] — el ternario como expresión en el lado derecho de una asignación
