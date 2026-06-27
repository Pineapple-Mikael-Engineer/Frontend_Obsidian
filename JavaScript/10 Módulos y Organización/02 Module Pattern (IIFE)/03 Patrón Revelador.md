---
title: Patrón Revelador — Revealing Module Pattern
aliases:
  - Revealing Module Pattern
  - patrón revelador
  - revealing pattern
tags:
  - javascript
  - api/concepto
  - modulos
draft: false
order: 3
---

# Patrón Revelador

> [!definicion]
> El **Revealing Module Pattern** (Patrón Revelador) es una variante del Module Pattern donde toda la lógica — tanto privada como pública — se define de forma uniforme en el scope privado de la IIFE. Al final, el objeto retornado solo **revela** (referencia) las funciones que deben ser públicas. No hay mezcla de definición-dentro-del-objeto-retornado con definición-en-el-scope: todo está definido igual, la diferencia es qué se incluye en el `return`.

```js
const moduloUsuario = (function() {
  let _nombre = "";
  let _email = "";

  function _validarEmail(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  }

  function setNombre(nombre) {
    if (!nombre || nombre.trim() === "") throw new Error("Nombre vacío");
    _nombre = nombre.trim();
  }

  function setEmail(email) {
    if (!_validarEmail(email)) throw new Error("Email inválido");
    _email = email;
  }

  function getPerfil() {
    return { nombre: _nombre, email: _email };
  }

  // Solo se revelan las funciones públicas
  return { setNombre, setEmail, getPerfil };
})();

moduloUsuario.setNombre("Ana");
moduloUsuario.setEmail("ana@ejemplo.com");
moduloUsuario.getPerfil(); // { nombre: "Ana", email: "ana@ejemplo.com" }
moduloUsuario._validarEmail; // undefined — privada
```

## Diferencia con el Module Pattern básico

En el Module Pattern básico las funciones públicas se definen **dentro** del objeto literal retornado; las privadas, fuera. En el Patrón Revelador, **todas** se definen fuera y el `return` solo mapea nombres:

```js
// Module Pattern básico — mezcla definición en objeto retornado
const contadorBasico = (function() {
  let _n = 0;
  function _doblar() { return _n * 2; }

  return {
    incrementar() { _n++; },    // definida aquí dentro
    doblar()      { return _doblar(); },
    valor()       { return _n; }
  };
})();

// Patrón Revelador — todo definido igual, return solo revela
const contadorRevelador = (function() {
  let _n = 0;

  function _doblar()    { return _n * 2; }
  function incrementar(){ _n++; }
  function doblar()     { return _doblar(); }
  function valor()      { return _n; }

  return { incrementar, doblar, valor };
})();
```

El Patrón Revelador es más legible en módulos grandes: al leer el `return` se ve de inmediato qué es público sin buscar entre definiciones de métodos.

## Ejemplo completo: módulo de autenticación

```js
const Auth = (function() {
  let _token = null;
  let _usuario = null;

  function _decodificarToken(token) {
    // Simplificación — en producción: parsear JWT
    const [, payload] = token.split(".");
    return JSON.parse(atob(payload));
  }

  function login(token) {
    _token = token;
    _usuario = _decodificarToken(token);
  }

  function logout() {
    _token = null;
    _usuario = null;
  }

  function estaAutenticado() {
    return _token !== null;
  }

  function getUsuario() {
    if (!estaAutenticado()) throw new Error("No autenticado");
    return { ..._usuario }; // copia defensiva
  }

  return { login, logout, estaAutenticado, getUsuario };
})();

Auth.login("eyJ...token...");
Auth.estaAutenticado(); // true
Auth.getUsuario();      // { sub: "123", name: "Ana", ... }
Auth._token;            // undefined
```

## Desventaja: referencias internas fijas

La principal limitación del Patrón Revelador afecta a los overrides externos (comunes en tests):

```js
const moduloTest = (function() {
  function privada() { return "original"; }
  function publica() { return privada(); } // referencia directa a privada

  return { publica };
})();

// Intento de monkey-patching desde fuera
moduloTest.publica = function() { return "mock"; };

// Pero si otra función pública llama a publica() internamente,
// la referencia interna sigue apuntando a la original — no al mock
```

Esto ocurre porque las funciones internas se llaman entre sí por sus nombres en el scope privado, no a través del objeto retornado. En el Module Pattern básico, si las funciones públicas se llaman a través de `this` dentro del objeto, el override funciona; en el Patrón Revelador no.

## Cuándo usar cada variante

| Criterio | Module Pattern básico | Patrón Revelador |
|---|---|---|
| Muchas funciones privadas | Mezcla visible en el return | Más limpio — todo definido igual |
| Necesidad de override/mock | Funciona si se usa `this` | Frágil — referencias internas fijas |
| Legibilidad del contrato público | Hay que leer el return completo | El return es declarativo e inmediato |
| Código legacy a mantener | Patrón más frecuente | Menos frecuente pero más ordenado |

> [!tip]
> El Patrón Revelador brilla en módulos medianos con varias funciones donde la distinción pública/privada es el aspecto más importante. Para código nuevo, ESM (`export function`) ofrece el mismo nivel de claridad con tree-shaking y soporte de herramientas.

> [!warning]
> Si el módulo necesita ser testeado con mocks o si funciones públicas se llaman entre sí internamente, considerar el Module Pattern básico (donde las llamadas internas pasan por el objeto `this`) o directamente ESM con funciones exportadas e importadas explícitamente. El Patrón Revelador no es amigable con frameworks de testing que hacen spy sobre métodos del objeto retornado.

## Comparación con ESM

El Patrón Revelador resuelve manualmente lo que ESM da de forma nativa:

```js
// Patrón Revelador
const M = (function() {
  function privada() { /* ... */ }
  function publica() { return privada(); }
  return { publica };
})();

// ESM equivalente — más limpio, con tree-shaking
function _privada() { /* ... */ }
export function publica() { return _privada(); }
```

En ESM, todo lo no exportado es privado al archivo sin necesidad de IIFE ni closure manual. El bundler puede además eliminar `publica` si nunca se importa (tree-shaking), cosa imposible con el Patrón Revelador.

## Notas relacionadas

- [[02 Module Pattern (IIFE)/index | Module Pattern (IIFE)]]
- [[02 Module Pattern (IIFE)/01 IIFE | IIFE]]
- [[02 Module Pattern (IIFE)/02 Encapsulación con Closures | Encapsulación con Closures]]
- [[01 Módulos ES6/index | Módulos ES6 (ESM)]]
