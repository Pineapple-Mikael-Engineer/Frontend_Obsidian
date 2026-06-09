---
title: Memoización
aliases:
  - memoize
  - caché de función
  - function caching
tags:
  - javascript
  - api/concepto
  - patrones
draft: false
---

# Memoización

> [!definicion]
> La **memoización** es una técnica de optimización que almacena en caché los resultados de una función para cada combinación de argumentos, de modo que llamadas posteriores con los mismos argumentos devuelven el resultado cacheado sin volver a computar. Solo es correcta para **funciones puras**: mismo input siempre produce el mismo output y sin efectos secundarios.

## Implementación general

```js
function memoize(fn) {
  const cache = new Map();
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const resultado = fn.apply(this, args);
    cache.set(key, resultado);
    return resultado;
  };
}
```

```js
function fibonacci(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

const fibMemo = memoize(fibonacci);
fibMemo(40); // calcula: ~1 ms
fibMemo(40); // caché: < 0.01 ms
```

> [!warning]
> La función `fibonacci` recursiva se llama a sí misma, no a `fibMemo`. Para que la memoización afecte también a las llamadas recursivas internas, la función debe referenciarse a través de la versión memoizada desde el inicio.

## Cuándo aplicar

Memoizar es beneficioso cuando las tres condiciones se cumplen simultáneamente:

1. La función es **pura** — sin efectos secundarios, sin dependencia de estado externo mutable.
2. El cálculo es **costoso** (complejidad alta, algoritmo recursivo, transformación de datos pesada).
3. La función se llama **repetidamente** con los mismos argumentos durante la vida de la aplicación.

No memoizar: funciones que leen de la red, acceden a la fecha/hora, generan números aleatorios, o modifican el DOM.

## Limitaciones de `JSON.stringify` como clave

| Caso | Comportamiento |
|---|---|
| Primitivos (number, string, boolean) | Correcto |
| Arrays y objetos simples | Correcto (orden de propiedades puede variar en objetos) |
| Objetos con referencias circulares | Lanza `TypeError` |
| Funciones como argumento | Se serializan como `undefined` — claves incorrectas |
| `undefined` como argumento | Se serializa como `null` en arrays, omitido en objetos |
| Clases con métodos | Solo se serializan las propiedades enumerables |

## Memoización basada en `WeakMap` para objetos

Para funciones que reciben un único argumento objeto, `WeakMap` evita serialización y elimina memory leaks: las keys son débiles y se recolectan con el garbage collector cuando el objeto ya no tiene otras referencias.

```js
function memoizeWeakMap(fn) {
  const cache = new WeakMap();
  return function(obj) {
    if (cache.has(obj)) return cache.get(obj);
    const resultado = fn.call(this, obj);
    cache.set(obj, resultado);
    return resultado;
  };
}

const procesarUsuario = memoizeWeakMap(usuario => {
  return { ...usuario, nombreCompleto: `${usuario.nombre} ${usuario.apellido}` };
});
```

`WeakMap` solo acepta objetos como claves — no sirve para primitivos ni múltiples argumentos sin estructura de clave compuesta.

## Crecimiento ilimitado de la caché

La implementación básica con `Map` nunca limpia. En aplicaciones de larga duración con muchos inputs distintos, la caché crece indefinidamente. Las estrategias de control:

**LRU cache** (Least Recently Used): descarta las entradas menos recientemente usadas cuando se alcanza un límite de tamaño. Librerías como `lru-cache` implementan esto de forma eficiente.

```js
// Con límite manual simple (FIFO, no LRU)
function memoizeConLimite(fn, limite = 100) {
  const cache = new Map();
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    if (cache.size >= limite) {
      const primeraKey = cache.keys().next().value;
      cache.delete(primeraKey); // elimina la entrada más antigua
    }
    const resultado = fn.apply(this, args);
    cache.set(key, resultado);
    return resultado;
  };
}
```

## Memoización en React

| API | Qué memoiza | Cuándo |
|---|---|---|
| `React.memo(Componente)` | El componente completo | Evita re-renders cuando las props no cambian |
| `useMemo(fn, deps)` | El valor devuelto por `fn` | Cálculo costoso dentro de un componente |
| `useCallback(fn, deps)` | La referencia a la función | Pasar handlers estables a componentes hijos |

`useMemo` y `useCallback` invalidan automáticamente la caché cuando cambian las dependencias del array `deps`.

## Cómo funciona por dentro

La función envolvente devuelta por `memoize` cierra sobre la variable `cache` mediante un closure. Cada llamada computa la clave, consulta el `Map` en O(1) amortizado, y solo ejecuta `fn` si no hay hit. El uso de `fn.apply(this, args)` preserva el contexto `this` del caller, lo que permite memoizar métodos de clase sin perder la referencia al objeto receptor.

> [!tip]
> El candidato ideal para memoización en frontend: una función de transformación de datos (formatear listas, calcular totales, filtrar grandes arrays) que se ejecuta en cada re-render y recibe siempre el mismo objeto de datos hasta que el usuario realiza una acción. Memoizarla elimina trabajo redundante sin cambiar el comportamiento observable.

> [!warning]
> Memoizar funciones impuras produce resultados incorrectos silenciosamente: si la función depende de estado externo (`Date.now()`, una variable global, el DOM), la caché devuelve un resultado obsoleto sin indicación de error. Antes de memoizar, verificar que la función satisface la condición de pureza: mismo input → mismo output, siempre.

## Notas relacionadas

- [[03 IIFE|IIFE]]
- [[05 Patrones de Diseño/index|Patrones de Diseño]]
- [[06 Separación de Responsabilidades|Separación de Responsabilidades]]
