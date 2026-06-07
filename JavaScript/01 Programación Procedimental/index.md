---
title: Programación Procedimental en JavaScript
aliases:
  - js procedimental
  - fundamentos javascript
tags:
  - javascript
  - api/concepto
  - sintaxis
draft: false
---

# Programación Procedimental en JavaScript

> [!definicion]
> La **programación procedimental** es el paradigma base de JavaScript: instrucciones que se ejecutan en secuencia, variables que almacenan estado, condicionales que bifurcan el flujo y bucles que lo repiten. Es el fundamento sobre el que se construyen los paradigmas de orientación a objetos y programación funcional.

## Qué cubre esta sección

| Subsección | Contenido |
|---|---|
| 01 Sintaxis Básica | Declaraciones vs expresiones, punto y coma, comentarios, modo estricto |
| 02 Variables | var / let / const, hoisting, TDZ, ámbito, nombrado |
| 03 Tipos de Datos | Primitivos, typeof, instanceof, conversión de tipos |
| 04 Operadores | Asignación, aritméticos, comparación, lógicos, nullish, optional chaining |
| 05 Condicionales | if/else, switch, ternario |
| 06 Bucles | while, for, for…of, for…in, break/continue |
| 07 Funciones (básico) | Declaración, expresión, parámetros, rest, return, primera clase |

## El modelo de ejecución

JavaScript es **single-threaded** y **síncrono** en su núcleo procedimental: una instrucción a la vez, de arriba a abajo. El motor V8 (u otro) pasa por dos fases antes de ejecutar cualquier archivo:

1. **Compilación / análisis**: el parser recorre el código, detecta errores de sintaxis y registra las declaraciones (hoisting).
2. **Ejecución**: las instrucciones se ejecutan en orden, en el contexto de ejecución creado para ese ámbito.

Entender estas dos fases explica el hoisting, la TDZ, y por qué las `function` declarations están disponibles antes de su línea.

## Notas relacionadas

- [[02 Programación Orientada a Objetos/index]] — el siguiente paradigma, construido sobre estos fundamentos.
- [[03 Programación Funcional/index]] — funciones como ciudadanos de primera clase.
- [[07 Programación Asíncrona/index]] — cuando el flujo deja de ser estrictamente secuencial.
