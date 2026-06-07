---
title: Currying — Transformar funciones en cadenas unarias
aliases:
  - currying
  - función currificada
  - curry
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
---

# Currying

> [!definicion]
> El **currying** transforma una función de múltiples argumentos en una secuencia de funciones unarias (de un solo argumento). `f(a, b, c)` se convierte en `f(a)(b)(c)`: cada llamada recibe un argumento y devuelve una nueva función que espera el siguiente, hasta que se acumulan todos los argumentos necesarios y se ejecuta la función original.

```js
// Sin currying
const sumar = (a, b) => a + b;
sumar(3, 5);  // 8

// Con currying
const sumarC = a => b => a + b;
sumarC(3)(5);   // 8

// Especializar: fijar el primer argumento
const sumar10 = sumarC(10);
sumar10(3);  // 13
sumar10(7);  // 17
```

## Implementación manual

Para funciones de aridad 2 y 3, la implementación manual es la más clara y eficiente:

```js
// Aridad 2
const multiplicar = a => b => a * b;
const doble = multiplicar(2);
const triple = multiplicar(3);

doble(5);   // 10
triple(5);  // 15

// Aridad 3
const clamp = min => max => valor => Math.min(Math.max(valor, min), max);
const entre0y100 = clamp(0)(100);

entre0y100(150); // 100
entre0y100(-5);  // 0
entre0y100(42);  // 42
```

## Curry automático (aridad arbitraria)

Para currificar cualquier función existente de forma genérica:

```js
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    }
    return function(...args2) {
      return curried.apply(this, args.concat(args2));
    };
  };
}

const sumarTres = (a, b, c) => a + b + c;
const sumarTresC = curry(sumarTres);

sumarTresC(1)(2)(3);   // 6
sumarTresC(1, 2)(3);   // 6  — admite acumulación parcial
sumarTresC(1)(2, 3);   // 6
sumarTresC(1, 2, 3);   // 6  — puede llamarse completa
```

La implementación comprueba si el número de argumentos acumulados (`args.length`) alcanza la aridad de la función original (`fn.length`). Si sí, invoca la función; si no, devuelve una nueva función que combina los argumentos acumulados con los nuevos.

## Casos de uso: predicados y transformadores configurables

### Filtros configurables con `filter`

```js
const mayorQue = umbral => num => num > umbral;
const menorQue = umbral => num => num < umbral;

const nums = [1, 5, 10, 20, 3, 8];
nums.filter(mayorQue(5));   // [10, 20, 8]
nums.filter(menorQue(5));   // [1, 3]
```

### Transformadores configurables con `map`

```js
const multiplicarPor = factor => n => n * factor;
const aplicarDescuento = pct => precio => precio * (1 - pct / 100);

const precios = [100, 250, 80];
precios.map(multiplicarPor(1.21));      // [121, 302.5, 96.8] — con IVA
precios.map(aplicarDescuento(15));      // [85, 212.5, 68] — 15% descuento
```

### Validaciones parametrizadas

```js
const longitudMinima = min => str => str.length >= min;
const contiene = patron => str => str.includes(patron);

const validarPassword = [
  longitudMinima(8),
  contiene("@"),
  contiene("!"),
];

const password = "Hola@mundo!";
validarPassword.every(fn => fn(password)); // true
```

## Diferencia con Partial Application

| Aspecto | Currying | Partial Application |
|---|---|---|
| Aridad de cada retorno | Siempre 1 (unaria) | Puede ser cualquiera |
| Argumentos por llamada | Uno a la vez | Varios a la vez posible |
| Objetivo | Secuencia de unarias | Fijar N argumentos iniciales |

Un ejemplo que ilustra la diferencia:

```js
// Currying: cada llamada recibe exactamente 1 argumento
const currificada = a => b => c => a + b + c;
currificada(1)(2)(3);  // 6

// Partial Application: puede fijar varios a la vez
function partial(fn, ...init) {
  return (...rest) => fn(...init, ...rest);
}
const conA1B2 = partial((a, b, c) => a + b + c, 1, 2);
conA1B2(3);  // 6 — fijó a y b en una sola llamada
```

## Cómo funciona por dentro

Cada llamada a una función currificada devuelve un **closure** que captura los argumentos acumulados en su ámbito. Cuando se invoca la siguiente función, combina los argumentos capturados con los nuevos. El proceso continúa hasta que el número total de argumentos acumulados iguala o supera la aridad de la función original (`fn.length`), momento en que se invoca con todos los argumentos.

```js
// Traza de ejecución
const sumarC = curry((a, b, c) => a + b + c);

const paso1 = sumarC(10);
// args = [10], 1 < 3 → retorna closure que captura [10]

const paso2 = paso1(20);
// args = [10, 20], 2 < 3 → retorna closure que captura [10, 20]

paso2(30);
// args = [10, 20, 30], 3 >= 3 → invoca fn(10, 20, 30) → 60
```

> [!tip] Buenas prácticas
> - Diseñar funciones currificadas con los argumentos "más estables" primero y los "más variables" al final — el argumento que varía más es el último, de modo que la especialización parcial sea natural.
> - Para funciones genéricas de utilidad (filtros, transformadores, validadores), el currying permite crear familias de funciones especializadas desde una sola función base.
> - Preferir la implementación manual (`a => b => ...`) para funciones de aridad conocida: más legible y más eficiente que el curry automático genérico.

> [!warning] Errores comunes
> - **`fn.length` no cuenta parámetros rest ni con valor por defecto:** `function f(a, b = 0) {}` tiene `f.length === 1`, no 2. El curry automático puede no comportarse como se espera con estas firmas.
> - **Mezclar currying con funciones que usan `this`:** los closures de currying no preservan `this` automáticamente. Si la función original necesita `this`, usar `bind` o arrow functions con cuidado.
> - **Llamar con 0 argumentos:** `sumarC()` con la implementación genérica devuelve otra función en lugar de ejecutar, porque `0 < fn.length`. Verificar la aridad esperada.

## Notas relacionadas

- [[index|Currying y Composición — Índice]]
- [[02 Partial Application]]
- [[03 compose]]
- [[04 pipe]]
