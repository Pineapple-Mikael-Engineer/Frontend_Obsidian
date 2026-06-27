---
title: Labeled Statements — Etiquetas para saltos en bucles anidados
aliases:
  - labeled statements
  - etiquetas de bucle
  - break con etiqueta
  - continue con etiqueta
tags:
  - javascript
  - api/concepto
  - control-flujo
objeto: global
tipo: concepto
draft: false
order: 8
---

# Labeled Statements

> [!definicion]
> Una **labeled statement** asigna un identificador (etiqueta) a un bloque o bucle, permitiendo que `break etiqueta` salga de ese bloque/bucle y `continue etiqueta` salte a la siguiente iteración de ese bucle, desde cualquier nivel anidado interior. Sintaxis: `etiqueta: instrucción` + `break etiqueta` / `continue etiqueta`.

```js
externo: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) break externo; // sale de AMBOS bucles
    console.log(i, j);
  }
}
// 0 0 → 0 1 → 0 2 → 1 0
// Al llegar a i=1, j=1: break externo → sale de todo
```

## Cómo funciona por dentro

El parser registra la etiqueta y la asocia a la instrucción que le sigue. Cuando el motor encuentra `break etiqueta`, busca en la pila de bucles activos el que tiene esa etiqueta y transfiere el control a la instrucción posterior a ese bucle. Para `continue etiqueta`, transfiere el control al paso de actualización (en `for`) o a la evaluación de la condición (en `while`) del bucle etiquetado.

Las etiquetas no son `goto`: no pueden saltar a lugares arbitrarios del código. Solo pueden referenciar el bucle o bloque al que pertenecen, y solo desde el interior de ese bloque.

## `break etiqueta` — salir de bucles anidados

El caso de uso más frecuente: búsqueda en una estructura bidimensional.

```js
const matriz = [
  [1,  2,  3],
  [4,  5,  6],
  [7,  8,  9],
];
const objetivo = 5;
let encontrada = false;

busqueda: for (let i = 0; i < matriz.length; i++) {
  for (let j = 0; j < matriz[i].length; j++) {
    if (matriz[i][j] === objetivo) {
      console.log(`Encontrado en [${i}][${j}]`); // "Encontrado en [1][1]"
      encontrada = true;
      break busqueda; // sale de ambos bucles
    }
  }
}
console.log("Fin de la búsqueda"); // se ejecuta tras el break
```

Sin etiqueta, `break` solo saldría del bucle interno, y el bucle externo continuaría iterando filas aunque ya se hubiera encontrado el valor.

## `continue etiqueta` — saltar a la siguiente iteración del bucle externo

```js
externo: for (let i = 0; i < 4; i++) {
  for (let j = 0; j < 4; j++) {
    if (j === 2) continue externo; // salta al siguiente i, no al siguiente j
    console.log(i, j);
  }
}
// 0 0 → 0 1 → 1 0 → 1 1 → 2 0 → 2 1 → 3 0 → 3 1
```

## Etiqueta en bloque (sin bucle)

`break etiqueta` también puede salir de un bloque `{}` arbitrario etiquetado, no solo de bucles. `continue etiqueta` solo funciona con bucles.

```js
procesamiento: {
  const resultado = calcular();
  if (resultado === null) break procesamiento; // sale del bloque
  const transformado = transformar(resultado);
  if (!valido(transformado)) break procesamiento;
  guardar(transformado);
}
// El break en cualquier punto transfiere el control aquí
```

Este patrón aparece en código generado o en optimizaciones de salida temprana de bloques con múltiples condiciones.

## Cuándo son necesarias las etiquetas

En código moderno, las labeled statements señalan a menudo que el código puede refactorizarse. La alternativa más limpia es encapsular la búsqueda en una función y usar `return`:

```js
// Con etiqueta
function buscarConEtiqueta(matriz, objetivo) {
  busqueda: for (let i = 0; i < matriz.length; i++) {
    for (let j = 0; j < matriz[i].length; j++) {
      if (matriz[i][j] === objetivo) {
        break busqueda;
      }
    }
  }
}

// Equivalente con return — más claro, más testeable
function buscar(matriz, objetivo) {
  for (let i = 0; i < matriz.length; i++) {
    for (let j = 0; j < matriz[i].length; j++) {
      if (matriz[i][j] === objetivo) return { i, j };
    }
  }
  return null;
}
```

La versión con `return` es preferible: elimina la etiqueta, devuelve el resultado directamente y es más fácil de testear. Las etiquetas son legítimas cuando no se puede refactorizar a función (contexto de generadores, código embebido, o cuando el bucle externo tiene estado compartido complejo).

## Reglas de la especificación

- El nombre de la etiqueta es un identificador JavaScript válido (no una palabra reservada).
- Una etiqueta puede aparecer solo antes de una instrucción — no antes de una declaración de variable.
- `continue etiqueta` requiere que la instrucción etiquetada sea un bucle.
- Las etiquetas tienen su propio espacio de nombres: no colisionan con variables ni funciones.

> [!tip] Buenas prácticas
> - Antes de usar una etiqueta, evaluar si refactorizar el bucle anidado en una función con `return` es más claro.
> - Si se usa una etiqueta, nombrarla con la semántica de la operación (`busqueda:`, `parseo:`, `externo:`) — no simplemente `L1:` o `loop:`.
> - Documentar el `break etiqueta` con un comentario cuando la etiqueta no sea inmediatamente visible.

> [!warning] Errores comunes
> **Confundirlas con `goto`** — las etiquetas no saltan a ningún lugar arbitrario del código. Solo actúan sobre el bucle o bloque al que pertenecen. No existe `goto` en JavaScript.
> **`continue` en un bloque no-bucle** — `continue etiqueta` sobre una etiqueta que apunta a un bloque (no a un bucle) es un `SyntaxError`.
> **Etiqueta fuera del alcance** — referenciar una etiqueta de un bloque ya cerrado es un `SyntaxError`: las etiquetas solo son visibles dentro del bloque que etiquetan.

## Notas relacionadas

- [[06 Bucles/index | Bucles]]
- [[07 break y continue]]
- [[03 for Clásico]]
- [[05 for...of]]
