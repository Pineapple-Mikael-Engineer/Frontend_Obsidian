---
title: Conceptos Fundamentales de Programación Funcional
aliases:
  - FP conceptos
  - programación funcional JS
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
order: 1
---

# Conceptos Fundamentales de Programación Funcional

> [!definicion]
> La **programación funcional** (FP) es un paradigma que trata la computación como evaluación de funciones matemáticas: evita el estado mutable y los efectos secundarios, y prefiere componer funciones pequeñas y puras para construir comportamiento complejo. JavaScript no es un lenguaje de FP puro — es FP pragmático mezclado con imperativo y orientación a objetos.

## Los cinco pilares

| Concepto | Idea central | Herramienta JS principal |
|---|---|---|
| Funciones puras | Mismo input → mismo output, sin efectos | Funciones ordinarias sin acceso externo |
| Inmutabilidad | No mutar valores; producir nuevas versiones | Spread, métodos `to*` (ES2023), `Object.freeze` |
| Control de efectos secundarios | Aislar I/O y mutaciones en los bordes del sistema | Inyección de dependencias, functional core |
| Composición de funciones | Encadenar funciones donde la salida de una es la entrada de la siguiente | `pipe`, `compose`, method chaining |
| Estilo declarativo | Describir qué se quiere, no cómo hacerlo | `map`, `filter`, `reduce`, `find` |

## Por qué FP en JavaScript

JavaScript trata las funciones como **ciudadanos de primera clase**: se pueden asignar a variables, pasar como argumentos y retornar desde otras funciones. Esto hace que FP no sea una adaptación forzada sino una forma natural de escribir código JS moderno. Las APIs de array (`map`, `filter`, `reduce`), las funciones flecha y los métodos `to*` de ES2023 son señales de que el lenguaje evoluciona en esta dirección.

FP pragmático no exige pureza total — exige **separar el núcleo puro de los bordes impuros**: la lógica de negocio vive en funciones sin efectos; las llamadas a la red, el DOM y la base de datos quedan en la periferia.

## Modelo mental

```
Entrada → [función pura] → Salida nueva
                ↑
        sin estado compartido
        sin mutaciones
        sin I/O directo
```

El código funcional es un pipeline de transformaciones: los datos entran, se procesan a través de funciones compuestas y salen transformados. El estado anterior nunca se destruye — se produce uno nuevo.

## Notas relacionadas

- [[01 Funciones Puras]]
- [[02 Inmutabilidad]]
- [[03 Efectos Secundarios]]
- [[04 Composición de Funciones]]
- [[05 Declarativo vs Imperativo]]
- [[02 Programación Orientada a Objetos/index|Programación Orientada a Objetos]] — el otro paradigma mezclado con FP en JS.
