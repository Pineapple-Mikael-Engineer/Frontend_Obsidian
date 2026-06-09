---
title: Depuración — mapa de la sección
aliases:
  - debugging javascript
  - depuración js
tags:
  - javascript
  - api/concepto
  - errores
objeto: DevTools
tipo: herramienta
retorna: "-"
muta: false
asincrono: false
draft: false
---

# Depuración

> [!definicion]
> La depuración en JavaScript se apoya en dos pilares: `console` para logging estructurado sin detener la ejecución, y el debugger (sentencia `debugger` + breakpoints en DevTools) para pausar e inspeccionar el estado del programa en cualquier punto del call stack. Combinados permiten diagnosticar bugs desde trazas de alto nivel hasta inspección de variables en tiempo de ejecución.

## Contenido de la sección

| Nota | Qué cubre |
|------|-----------|
| 01 Métodos de console | Todos los métodos de `console`: log, warn, error, table, group, time, assert, trace |
| 02 debugger y Breakpoints | Sentencia `debugger`, tipos de breakpoints en DevTools (line, conditional, logpoint, DOM, XHR, exception) |
| 03 Step y Watch Expressions | Acciones de step (F8/F10/F11), Scope panel, Call Stack, Watch Expressions, evaluate en consola |

## Flujo de depuración típico

1. Reproducir el bug de forma consistente.
2. Añadir `console.log` o `console.table` para acotar el área problemática.
3. Poner un breakpoint (o `debugger`) en el punto sospechoso.
4. Usar Step Over / Step Into para seguir la ejecución línea a línea.
5. Inspeccionar variables en el Scope panel y Watch Expressions.
6. Evaluar expresiones ad-hoc en la consola (tiene acceso al scope actual).

## Notas relacionadas

- [[04 Depuración/01 Métodos de console | Métodos de console]]
- [[04 Depuración/02 debugger y Breakpoints | debugger y Breakpoints]]
- [[04 Depuración/03 Step y Watch Expressions | Step y Watch Expressions]]
- [[02 try - catch - finally/index | try / catch / finally]]
- [[03 throw/index | throw y errores personalizados]]
