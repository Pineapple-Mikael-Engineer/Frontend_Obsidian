---
title: Modificar Atributos del DOM — getAttribute, setAttribute, classList, dataset
aliases:
  - Modificar Atributos
  - DOM attribute manipulation
tags:
  - javascript
  - api/concepto
  - dom
objeto: Element
tipo: concepto
muta: true
asincrono: false
draft: false
---

# Modificar Atributos del DOM

> [!definicion]
> Los atributos HTML de un elemento se manipulan a través de dos interfaces: los **métodos genéricos** (`getAttribute`, `setAttribute`, `removeAttribute`, `hasAttribute`) que operan sobre cualquier atributo como pares string-string, y las **propiedades directas** del objeto DOM (`el.id`, `el.href`, `el.value`) que son propiedades IDL tipadas y a veces divergen del atributo HTML subyacente.

La distinción clave: los métodos genéricos trabajan con el **atributo HTML** (string, estado inicial del markup); las propiedades directas trabajan con el **estado actual** del elemento en memoria, que puede diferir tras la interacción del usuario.

## Tabla comparativa

| Método / Propiedad | Lee | Escribe | Elimina | Comprueba existencia |
|--------------------|-----|---------|---------|---------------------|
| `getAttribute(nombre)` | Sí — string o `null` | No | No | No |
| `setAttribute(nombre, valor)` | No | Sí | No | No |
| `removeAttribute(nombre)` | No | No | Sí | No |
| `hasAttribute(nombre)` | No | No | No | Sí — boolean |
| Propiedad directa (`el.id`) | Sí — tipada | Sí — tipada | No (asignar `""`) | No |
| `classList` | Sí — DOMTokenList | Sí (add/remove/toggle) | Sí (remove) | Sí (contains) |
| `dataset` | Sí — camelCase | Sí — camelCase | Sí (`delete`) | Sí (`in`) |

## Cuándo usar cada interfaz

Los métodos genéricos son necesarios cuando el atributo no tiene propiedad directa equivalente (atributos personalizados sin `data-*`, atributos SVG, atributos ARIA), o cuando se necesita el valor original del markup antes de cualquier modificación por JS o usuario. Las propiedades directas son más ergonómicas para atributos estándar (`id`, `class`, `src`, `href`, `value`, `checked`, `disabled`). `classList` y `dataset` son las APIs modernas para los dos casos más frecuentes de atributos compuestos.

## Notas relacionadas

- [[01 getAttribute y setAttribute]]
- [[02 removeAttribute y hasAttribute]]
- [[03 Atributos Directos]]
- [[04 classList]]
- [[05 dataset]]
- [[05 Modificar Estilos/index | Modificar Estilos]]
