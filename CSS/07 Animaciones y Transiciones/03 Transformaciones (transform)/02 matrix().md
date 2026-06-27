---
title: matrix() — Todas las transformaciones en una matriz
aliases:
  - matrix
tags:
  - css
  - api/funcion
  - animacion
draft: false
order: 2
---

# matrix()

> [!definicion]
> `matrix(a, b, c, d, e, f)` combina **todas** las transformaciones 2D (translate, rotate, scale, skew) en una **matriz** de seis valores. Es la forma de bajo nivel a la que CSS reduce internamente cualquier combinación de transformaciones; rara vez se escribe a mano.

```css
transform: matrix(1, 0, 0, 1, 30, 20);   /* = translate(30px, 20px) */
```

## Qué representan los seis valores

> [!info] Una matriz de transformación 2D
> Los seis valores forman una matriz que define la transformación afín:
> ```
> matrix(a, b, c, d, e, f)
> a, d → escala (y rotación/skew)
> b, c → rotación y deformación (skew)
> e, f → traslación (x, y)
> ```
> | Equivalencia | matrix |
> |--------------|--------|
> | `translate(e, f)` | `matrix(1, 0, 0, 1, e, f)` |
> | `scale(sx, sy)` | `matrix(sx, 0, 0, sy, 0, 0)` |
> | `rotate(θ)` | `matrix(cos θ, sin θ, -sin θ, cos θ, 0, 0)` |

## Por qué casi nunca se escribe a mano

> [!warning] matrix() es ilegible; usa las funciones nombradas
> Escribir `matrix(0.866, 0.5, -0.5, 0.866, 0, 0)` en vez de `rotate(30deg)` es **incomprensible** y propenso a errores. Las funciones nombradas (`translate`, `rotate`, `scale`, `skew`) son infinitamente más legibles y se combinan igual de bien. **No** escribas `matrix()` a mano salvo casos muy específicos:
> ```css
> /* ❌ ilegible */
> transform: matrix(0.866, 0.5, -0.5, 0.866, 30, 20);
> /* ✅ claro */
> transform: rotate(30deg) translate(30px, 20px);
> ```

## Cuándo aparece matrix()

`matrix()` se ve sobre todo cuando:
- **JavaScript lee** una transformación con `getComputedStyle`: el navegador **siempre** devuelve la forma `matrix()`, no las funciones originales. Para leer la transformación actual de un elemento en JS, te encuentras una matriz.
- **Librerías de animación** la generan internamente por rendimiento.
- **`matrix3d()`** (la versión 3D, con 16 valores) en transformaciones tridimensionales complejas.

## matrix3d para 3D

La versión tridimensional, `matrix3d(...)`, tiene **16 valores** (una matriz 4×4) y combina todas las transformaciones 3D. Aún más ilegible; reservada para generación automática.

## Buenas prácticas

> [!tip] Recomendaciones
> - **No** escribas `matrix()`/`matrix3d()` a mano: usa las funciones nombradas.
> - Conócela para **entenderla** cuando JS te devuelve una matriz (`getComputedStyle`).
> - Si necesitas leer/manipular transformaciones en JS, usa `DOMMatrix` para parsearlas.

## Errores comunes

> [!warning] Trampas
> - **Escribir `matrix()` a mano**: ilegible y propenso a errores.
> - **Sorprenderse** de que JS devuelva `matrix()` en vez de `rotate()`: es lo normal.

## Notas relacionadas

- [[01 Transformaciones 2D (translate, rotate, scale, skew)]] — las funciones legibles que `matrix()` combina.
- [[03 Transformaciones 3D (translateZ, rotateX, scale3d)]] — donde aparece `matrix3d()`.
- [[index]] — `transform` en general.
