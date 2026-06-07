---
title: switch — Selector por igualdad estricta
aliases:
  - switch statement
  - sentencia switch
  - switch case
tags:
  - javascript
  - api/concepto
  - control-flujo
objeto: global
tipo: concepto
retorna: undefined
muta: false
asincrono: false
draft: false
---

# `switch`

> [!definicion]
> `switch` evalúa una expresión una sola vez y compara el resultado con cada `case` usando **igualdad estricta** (`===`). Ejecuta el bloque del primer `case` que coincida y continúa hasta encontrar un `break` o el fin del bloque. Si ningún `case` coincide, ejecuta `default` si existe.

```js
switch (expresión) {
  case valor1:
    // se ejecuta si expresión === valor1
    break;
  case valor2:
    // se ejecuta si expresión === valor2
    break;
  default:
    // se ejecuta si ningún case coincide
}
```

## El `break` es obligatorio — fall-through

Sin `break`, el motor no sale del bloque `switch` al terminar un `case`: **continúa ejecutando los `case` siguientes** aunque su valor no coincida con la expresión. Esto se llama **fall-through**.

```js
const dia = "lunes";

switch (dia) {
  case "lunes":
    console.log("inicio de semana"); // se ejecuta
    // falta break — cae al siguiente
  case "martes":
    console.log("segundo día");      // también se ejecuta (bug)
    break;
  case "miércoles":
    console.log("mitad de semana");  // no se ejecuta
    break;
}
// → "inicio de semana"
// → "segundo día"
```

El fall-through es **un error casi siempre**. ESLint `no-fallthrough` lo detecta. La excepción son los fall-throughs intencionales (ver sección siguiente).

> [!warning] Fall-through silencioso
> El bug más frecuente con `switch` es olvidar `break`. El código compila y ejecuta sin error; simplemente hace demasiado. Usar `/* falls through */` como comentario explícito cuando el fall-through es intencional para que ESLint no lo marque.

## Fall-through intencional — agrupar casos

Cuando varios valores deben ejecutar el mismo código, se apilan `case` consecutivos sin `break` entre ellos:

```js
function esDiaLaboral(dia) {
  switch (dia) {
    case "lunes":
    case "martes":
    case "miércoles":
    case "jueves":
    case "viernes":
      return true;
    case "sábado":
    case "domingo":
      return false;
    default:
      throw new Error(`Día desconocido: ${dia}`);
  }
}

esDiaLaboral("martes");  // → true
esDiaLaboral("domingo"); // → false
```

Este uso es idiomático y legible: los `case` vacíos actúan como alias del primero con código.

## `default` — el else del switch

`default` es opcional y puede colocarse en cualquier posición del bloque. Por convención va al final. El motor lo evalúa solo si ningún `case` coincide, independientemente de su posición.

```js
// default en el medio — válido pero no idiomático
switch (código) {
  case 200: return "OK";
  default:  return "Desconocido"; // evaluado si no hay match
  case 404: return "No encontrado";
  case 500: return "Error de servidor";
}
```

Omitir `default` es válido; en ese caso, si ningún `case` coincide, `switch` no ejecuta nada.

## Comparación con `===` — sin coerción

`switch` usa comparación estricta, equivalente a `===`. No hay coerción de tipos:

```js
const val = "1";

switch (val) {
  case 1:
    console.log("número 1"); // no coincide — "1" !== 1
    break;
  case "1":
    console.log("string 1"); // coincide
    break;
}
// → "string 1"
```

Esto también significa que `switch` no puede detectar `NaN`, porque `NaN !== NaN`:

```js
switch (NaN) {
  case NaN: console.log("es NaN"); // nunca coincide
}
```

Para `NaN`, usar `Number.isNaN()` dentro de un `if`.

## `switch(true)` — condiciones complejas

El truco consiste en pasar `true` como expresión del `switch` y escribir condiciones booleanas en cada `case`. La comparación `true === condición` coincide cuando la condición es truthy:

```js
function categoriaEdad(edad) {
  switch (true) {
    case edad < 0:
      throw new RangeError("Edad inválida");
    case edad < 13:
      return "niño";
    case edad < 18:
      return "adolescente";
    case edad < 65:
      return "adulto";
    default:
      return "adulto mayor";
  }
}
```

El patrón funciona pero **no es idiomático**. Para condiciones con rangos, una cadena `if/else if` es más legible. `switch(true)` solo aporta cuando hay muchas ramas de condición homogéneas.

## Alternativa moderna — objeto como tabla de dispatch

Para mapear valores discretos a acciones o datos, un objeto literal es más conciso y funcional que `switch`:

```js
// Con switch
function getIcono(estado) {
  switch (estado) {
    case "ok":      return "✅";
    case "error":   return "❌";
    case "pending": return "⏳";
    default:        return "❓";
  }
}

// Con objeto de dispatch — más conciso
const ICONOS = {
  ok:      "✅",
  error:   "❌",
  pending: "⏳",
};

const getIcono = (estado) => ICONOS[estado] ?? "❓";
```

Para acciones (funciones), el mismo patrón con optional chaining:

```js
const acciones = {
  click:     () => manejarClick(),
  hover:     () => manejarHover(),
  keydown:   (e) => manejarTecla(e),
};

// Invocar la acción si existe, sin hacer nada si no
acciones[evento.type]?.(evento);
```

La tabla de dispatch gana cuando los valores son conocidos en compilación y la lógica es simple. `switch` gana cuando las ramas tienen lógica compleja de varios pasos.

## Comparación `switch` vs `if/else if`

| Criterio | `switch` | `if/else if` |
|----------|----------|--------------|
| Tipo de comparación | Solo `===` | Cualquier expresión booleana |
| Expresión evaluada | Una sola vez | Una por rama |
| Rangos (`> 100`) | No | Sí |
| Múltiples variables por rama | No | Sí |
| Fall-through | Sí (requiere `break`) | No |
| Muchas ramas de igualdad | Más legible | Repetitivo |
| Optimización del motor | Jump table posible | Evaluación secuencial |

## Cómo funciona por dentro (motor)

El motor evalúa `expresión` exactamente una vez y almacena el resultado. Luego recorre los `case` en orden de aparición en el código fuente, comparando con `===`. Al encontrar el primer `case` que coincide, transfiere el control al inicio de ese bloque. El motor **no sale automáticamente** del `switch`: continúa ejecutando instrucción por instrucción (incluyendo los `case` siguientes) hasta encontrar `break`, `return`, `throw`, o el cierre `}` del bloque.

V8 optimiza `switch` con muchos `case` numéricos o de string consecutivos generando una **jump table** (tabla de salto) en lugar de comparaciones secuenciales, lo que reduce la complejidad de O(n) a O(1). Esto no se garantiza para `default` o `case` con expresiones no literales.

> [!tip] Buenas prácticas
> - Siempre incluir `break` al final de cada `case` con código, a menos que el fall-through sea intencional y esté comentado.
> - Siempre incluir `default`: como mínimo, lanzar un error o loggear el valor inesperado.
> - Para 2-3 ramas, `if/else` suele ser más legible. `switch` aporta a partir de ~4 ramas.
> - Para mapear valores a datos (no a bloques de código), preferir un objeto de dispatch.
> - Documentar el fall-through intencional con `/* falls through */`.

> [!warning] Errores comunes
> - **`break` olvidado**: el error más frecuente. ESLint `no-fallthrough` lo detecta.
> - **Coerción esperada**: `switch ("1")` no coincide con `case 1`. No hay coerción.
> - **`NaN` en el `case`**: `case NaN:` nunca coincide. Usar `if (Number.isNaN(x))` previo.
> - **`default` con fall-through**: si `default` no tiene `break` y no es el último, puede caer al siguiente `case`.
> - **Expresión compleja en `switch`**: `switch (a > b ? a : b)` funciona, pero oscurece la intención. Extraer a variable.

## Notas relacionadas

- [[05 Condicionales/index|Condicionales]] — comparativa de los tres mecanismos.
- [[01 if, else if, else]] — alternativa para condiciones no basadas en igualdad simple.
- [[03 Operador Ternario]] — versión expresión para selección binaria.
- [[04 Operadores/04 Lógicos|Lógicos]] — `&&`, `||`, `??` como alternativas condicionales cortas.
