---
title: Programación Orientada a Objetos en JavaScript
aliases:
  - POO javascript
  - OOP js
tags:
  - javascript
  - api/concepto
draft: false
---

# Programación Orientada a Objetos en JavaScript

> [!definicion]
> JavaScript implementa orientación a objetos a través de un **sistema prototípico**: cada objeto tiene un puntero interno `[[Prototype]]` a otro objeto, formando una cadena de delegación en tiempo de ejecución. No existen clases en el sentido de C++ o Java — la sintaxis `class` introducida en ES6 es azúcar sintáctico que compila a funciones constructoras y asignaciones de `prototype`. Todo el sistema de herencia, compartición de métodos y encapsulación se reduce al mismo mecanismo prototípico.

```js
// Todo lo siguiente usa el mismo mecanismo subyacente
const obj = { x: 1 };                      // objeto literal → hereda de Object.prototype
const arr = [1, 2, 3];                      // array → Array.prototype → Object.prototype
function Punto(x, y) { this.x = x; this.y = y; } // función constructora
class Rectangulo { constructor(w, h) { this.w = w; this.h = h; } } // azúcar

Object.getPrototypeOf(obj) === Object.prototype;        // true
Object.getPrototypeOf(arr) === Array.prototype;         // true
Object.getPrototypeOf(new Punto(0,0)) === Punto.prototype; // true
```

## Mapa de la sección

| Subsección | Qué cubre | Cuándo es relevante |
|---|---|---|
| 01 Objetos Literales | Creación `{}`, propiedades, métodos, getters/setters, recorrido, comprobación | Base de todo — siempre |
| 02 Prototipos | Cadena de prototipos, `Object.create`, funciones constructoras, herencia manual | Entender el motor; código pre-ES6; entrevistas |
| 03 Clases | `class`, `constructor`, métodos, campos `#`, herencia `extends`/`super` | Código moderno orientado a objetos |
| 04 this | Binding según contexto de invocación, `call`/`apply`/`bind`, pérdida de `this` | Siempre que haya métodos o callbacks |
| 05 Encapsulación y Abstracción | Closures como datos privados, Module Pattern, getters/setters de control | Diseño de APIs, código legacy, módulos pre-ES |

## Orden de aprendizaje recomendado

```
Objetos Literales → Prototipos → Clases → this → Encapsulación
     (01)              (02)        (03)     (04)        (05)
```

Este orden sigue la progresión histórica del lenguaje y el nivel de abstracción: primero el modelo de datos (objetos), luego el mecanismo de herencia (prototipos), luego la sintaxis de alto nivel que lo encapsula (clases), luego el contexto de invocación (this), y finalmente las técnicas de ocultamiento de estado (encapsulación).

## Subsecciones

- [[01 Objetos Literales/index | Objetos Literales]] — el tipo de dato central de JavaScript; propiedades, métodos, descriptores
- [[02 Prototipos/index | Prototipos]] — la cadena `[[Prototype]]`; herencia por delegación; `Object.create`; funciones constructoras
- [[03 Clases/index | Clases]] — sintaxis ES6; `constructor`, campos `#`, `extends`/`super`; equivalencia con prototipos
- [[04 this/index | this]] — binding por contexto de invocación; `call`/`apply`/`bind`; arrow functions; pérdida de contexto
- [[05 Encapsulación y Abstracción/index | Encapsulación y Abstracción]] — closures, Module Pattern, getters/setters para control de acceso

## Contexto histórico

JavaScript fue diseñado en 1995 con un modelo de objetos prototípico influenciado por Self, no por Java ni C++. La sintaxis de objetos con `{}` existía desde el principio; las funciones constructoras y `prototype` eran el único mecanismo de "clases" durante los primeros 20 años del lenguaje.

ES6 (2015) introdujo `class` como azúcar sintáctico — sin cambiar el motor — para facilitar la transición de desarrolladores con experiencia en lenguajes de clases clásicas. ES2022 añadió los campos privados `#`, el único mecanismo de encapsulación verdaderamente enforceado por el motor (antes, todo era convención de prefijo `_`).

El conocimiento del sistema prototípico sigue siendo esencial: `class` no oculta los prototipos, solo los automatiza. Cualquier problema de `this`, de herencia múltiple, de extensión de objetos nativos o de rendimiento de métodos apunta directamente al mecanismo de prototipos.
