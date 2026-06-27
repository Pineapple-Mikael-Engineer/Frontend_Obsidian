---
title: Closures Avanzados — Índice
aliases:
  - closures avanzados
  - closures JavaScript
tags:
  - javascript
  - api/concepto
  - patrones
draft: false
order: 2
---

# Closures Avanzados

> [!definicion]
> Un **closure** es la combinación de una función y el entorno léxico en el que fue definida: la función "recuerda" las variables del scope que la rodea aunque ese scope haya retornado. En patrones avanzados, este mecanismo habilita dos familias de uso: **fábricas de funciones** (funciones configuradas con comportamiento específico) y **datos privados** (estado encapsulado inaccesible desde fuera).

```js
// El closure más simple con consecuencias no triviales:
function crearContador() {
  let n = 0;          // vive en el closure, no en el llamador
  return () => ++n;
}

const c1 = crearContador();
const c2 = crearContador();

c1(); // 1  — instancias independientes
c1(); // 2
c2(); // 1  — c2 tiene su propio n
```

Cada llamada a `crearContador` crea un scope nuevo, y la función retornada captura una referencia a ese scope — no una copia del valor.

## Contenido de la sección

| # | Nombre | Cubre |
|---|---|---|
| 01 | Fábricas de Funciones | Funciones que retornan funciones configuradas; currying vs fábricas; vs clases |
| 02 | Datos Privados | Estado genuinamente privado; comparación con `#campos`; copia defensiva |

## Cómo se relacionan los conceptos

Las **fábricas** son la cara pública del patrón: crean y configuran funciones o módulos. Los **datos privados** son la cara de encapsulación: el closure como sustituto de modificadores de acceso. Ambos explotan el mismo mecanismo — que cada ejecución de una función crea un scope propio cuyas variables persisten mientras exista una referencia a la función interna.

## Notas relacionadas

- [[01 Fábricas de Funciones|Fábricas de Funciones]]
- [[02 Datos Privados|Datos Privados]]
