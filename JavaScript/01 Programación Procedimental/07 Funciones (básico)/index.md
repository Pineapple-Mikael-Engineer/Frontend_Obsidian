---
title: Funciones (básico) — Valores de primera clase
aliases:
  - funciones básico
  - functions basics
tags:
  - javascript
  - api/concepto
  - funciones
objeto: global
tipo: concepto
draft: false
order: 7
---

# Funciones (básico)

> [!definicion]
> Una **función** en JavaScript es un bloque de código reutilizable que encapsula una secuencia de instrucciones, puede recibir datos a través de parámetros y puede devolver un valor mediante `return`. Las funciones son **valores de primera clase**: se asignan a variables, se pasan como argumentos y se devuelven desde otras funciones, igual que números o strings.

El pipeline conceptual de cualquier llamada a función:

```js
function sumar(a, b) {   // parámetros formales
  return a + b;           // cuerpo → valor de retorno
}

const resultado = sumar(3, 4); // argumentos → 7
```

## Tres formas de definir una función

| Forma | Sintaxis | Hoisting | `this` propio | `arguments` |
|---|---|---|---|---|
| Declaración | `function f() {}` | completo (nombre + cuerpo) | sí | sí |
| Expresión | `const f = function() {}` | no (solo var → undefined) | sí | sí |
| Arrow | `const f = () => {}` | no | no (léxico) | no |

La elección entre declaración y expresión afecta principalmente al **hoisting** y al valor de `this`. Las arrow functions son un tipo de expresión con comportamiento diferenciado.

## El mecanismo de llamada

Cuando el motor encuentra `f(arg1, arg2)`:

1. Crea un nuevo **frame** (registro de activación) en la call stack.
2. Asigna los argumentos a los parámetros formales (o a `arguments`).
3. Ejecuta el cuerpo.
4. Al encontrar `return`, escribe el valor en el registro de retorno y desapila el frame.
5. El llamador lee ese valor de retorno.

Si no hay `return` explícito, la función devuelve `undefined`.

## Parámetros: los tres mecanismos

Los parámetros por defecto, rest parameters y el objeto `arguments` son tres mecanismos distintos para controlar los argumentos que recibe la función. No se mezclan libremente:

- `...rest` y `arguments` no se usan juntos (rest es el reemplazo moderno).
- Los defaults no cuentan en `fn.length`; los rest tampoco.

## Paso de argumentos

JavaScript pasa **siempre por valor**. Para primitivos, ese valor es el dato en sí. Para objetos, el valor es una referencia al objeto en el heap: la función puede mutar propiedades, pero no puede reasignar la variable del llamador.

## Funciones de primera clase

Que las funciones sean valores de primera clase habilita los patrones más expresivos del lenguaje: callbacks, composición, currying, memorización y closures. Estos patrones se desarrollan en profundidad en [[JavaScript/03 Programación Funcional/index | Programación Funcional]].

## Notas relacionadas

- [[01 Declaración de Función]]
- [[02 Expresión de Función]]
- [[03 Parámetros por Defecto]]
- [[04 Rest Parameters]]
- [[05 Objeto arguments]]
- [[06 Paso por Valor vs Referencia]]
- [[07 Return]]
- [[08 Funciones de Primera Clase]]
- [[JavaScript/01 Programación Procedimental/06 Bucles/index | Bucles]]
- [[JavaScript/02 Programación Orientada a Objetos/02 Prototipos/05 Funciones Constructoras (new, this) | Funciones Constructoras]]
- [[JavaScript/03 Programación Funcional/index | Programación Funcional]]
