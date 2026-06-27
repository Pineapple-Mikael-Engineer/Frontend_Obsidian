---
title: Selección de Elementos — Métodos del DOM
aliases:
  - selección de elementos
  - seleccionar nodos DOM
tags:
  - javascript
  - api/concepto
  - dom
objeto: document
tipo: concepto
muta: false
asincrono: false
draft: false
order: 1
---

# Selección de Elementos

> [!definicion]
> Seleccionar un elemento del DOM consiste en obtener una referencia a un nodo del árbol de documento para leerlo o manipularlo. La Web API expone cinco métodos principales, divididos en dos familias: los especializados (por `id`, clase, tag) y los basados en selectores CSS (`querySelector`/`querySelectorAll`).

```js
// Familia especializada
const el   = document.getElementById('app');
const cols = document.getElementsByClassName('col');
const divs = document.getElementsByTagName('div');

// Familia CSS — la más usada en código moderno
const first = document.querySelector('.card');
const all   = document.querySelectorAll('input[required]');
```

## Tabla comparativa

| Método | Contexto | Devuelve | Live / Static |
|---|---|---|---|
| `getElementById` | solo `document` | `Element` o `null` | — (nodo único) |
| `getElementsByClassName` | `document` o elemento | `HTMLCollection` | live |
| `getElementsByTagName` | `document` o elemento | `HTMLCollection` | live |
| `querySelector` | `document` o elemento | `Element` o `null` | static (nodo único) |
| `querySelectorAll` | `document` o elemento | `NodeList` | static |

## Regla práctica

Usar `querySelector` y `querySelectorAll` para la gran mayoría de casos: aceptan cualquier selector CSS válido y devuelven colecciones estáticas, más fáciles de iterar. Los métodos especializados (`getElementById`, `getElementsByClassName`, `getElementsByTagName`) son más rápidos —`getElementById` es O(1) por tabla hash interna— y su uso se justifica en rutas de código críticas o en código legado.

La distinción más importante entre familias no es la velocidad sino la naturaleza de la colección: `getElementsBy*` devuelve colecciones **live** (se actualizan automáticamente al mutar el DOM), mientras que `querySelectorAll` devuelve una **instantánea estática**.

## Contexto de elemento

`querySelector` y `getElementsBy*` están disponibles tanto en `document` como en cualquier `Element`. Al invocarlos sobre un elemento, la búsqueda se limita a sus descendientes:

```js
const form = document.querySelector('#login-form');
const inputs = form.querySelectorAll('input');   // solo dentro del formulario
```

## Notas relacionadas

- [[01 getElementById]]
- [[02 getElementsByClassName y TagName]]
- [[03 querySelector]]
- [[04 querySelectorAll]]
- [[05 HTMLCollection vs NodeList]]
