---
title: Errores Personalizados (extends Error)
aliases:
  - custom errors javascript
  - subclases de Error
  - extends Error
tags:
  - javascript
  - api/concepto
  - errores
objeto: Error
tipo: clase
retorna: Error
muta: false
asincrono: false
draft: false
order: 2
---

# Errores Personalizados (extends Error)

> [!definicion]
> Extender `Error` con `class MiError extends Error` permite crear tipos de error con semántica de dominio: nombres descriptivos, propiedades adicionales y discriminación con `instanceof`. Es el mecanismo estándar para modelar errores de aplicación que van más allá de los tipos nativos.

## Patrón base

```js
class ValidationError extends Error {
  constructor(campo, mensaje) {
    super(mensaje);          // pasa message a Error
    this.name = "ValidationError"; // sin esto, name sería "Error"
    this.campo = campo;
  }
}

class NotFoundError extends Error {
  constructor(recurso, id) {
    super(`${recurso} con id ${id} no encontrado`);
    this.name = "NotFoundError";
    this.statusCode = 404;
  }
}
```

```js
try {
  throw new ValidationError("email", "Formato inválido");
} catch (err) {
  console.log(err.name);    // → "ValidationError"
  console.log(err.campo);   // → "email"
  console.log(err.message); // → "Formato inválido"
  console.log(err instanceof ValidationError); // → true
  console.log(err instanceof Error);           // → true
}
```

## Por qué asignar `this.name` manualmente

Sin la línea `this.name = "NombreError"`, la propiedad `name` hereda el valor de `Error.prototype.name`, que es `"Error"`. El `stack trace` y los logs mostrarían `Error: ...` en lugar de `ValidationError: ...`, perdiendo el contexto semántico.

## Por qué `super(mensaje)` debe ir antes de `this`

En una clase derivada, el constructor de la superclase debe ejecutarse antes de poder acceder a `this`. Llamar a `super()` inicializa el objeto y registra el `stack trace` en ese punto. Intentar usar `this.name = ...` antes de `super(...)` lanza un `ReferenceError`.

## Discriminar con `instanceof`

```js
function manejarError(err) {
  if (err instanceof ValidationError) {
    return { status: 400, campo: err.campo, mensaje: err.message };
  }
  if (err instanceof NotFoundError) {
    return { status: 404, mensaje: err.message };
  }
  if (err instanceof AuthError) {
    return { status: 401, mensaje: "No autorizado" };
  }
  throw err; // re-lanzar errores no manejados
}
```

`instanceof` es preferible a `err.name === "ValidationError"` porque sobrevive a la cadena de prototipos completa y funciona con jerarquías de herencia.

## Jerarquía de errores de aplicación

```js
class AppError extends Error {
  constructor(mensaje, statusCode = 500) {
    super(mensaje);
    this.name = "AppError";
    this.statusCode = statusCode;
  }
}

class NetworkError extends AppError {
  constructor(mensaje) {
    super(mensaje, 503);
    this.name = "NetworkError";
  }
}

class AuthError extends AppError {
  constructor(mensaje) {
    super(mensaje, 401);
    this.name = "AuthError";
  }
}
```

Con esta jerarquía, `err instanceof AppError` captura cualquier error de la aplicación, mientras que `err instanceof NetworkError` discrimina el tipo específico.

## Caso especial: TypeScript y transpiladores (Babel)

Al transpilar clases ES6 a ES5 con Babel, `instanceof` puede fallar con subclases de `Error` porque Babel no puede reproducir fielmente la herencia de tipos nativos. La solución:

```ts
class ValidationError extends Error {
  constructor(campo: string, mensaje: string) {
    super(mensaje);
    this.name = "ValidationError";
    // Fix para Babel/TypeScript con target ES5
    Object.setPrototypeOf(this, new.target.prototype);
  }
}
```

`Object.setPrototypeOf(this, new.target.prototype)` restaura la cadena de prototipos correcta. En entornos modernos (TypeScript con `target: ES2015+` o Node nativo) no es necesario.

## Receta: jerarquía completa para una API REST

```js
class AppError extends Error {
  constructor(mensaje, statusCode = 500, codigo = "INTERNAL_ERROR") {
    super(mensaje);
    this.name = this.constructor.name; // usa el nombre de la subclase
    this.statusCode = statusCode;
    this.codigo = codigo;
  }
}

class ValidationError extends AppError {
  constructor(campo, mensaje) {
    super(mensaje, 400, "VALIDATION_ERROR");
    this.campo = campo;
  }
}

class NotFoundError extends AppError {
  constructor(recurso) {
    super(`${recurso} no encontrado`, 404, "NOT_FOUND");
    this.recurso = recurso;
  }
}

class AuthError extends AppError {
  constructor(mensaje = "No autorizado") {
    super(mensaje, 401, "UNAUTHORIZED");
  }
}
```

Usar `this.constructor.name` en `AppError` evita tener que asignar `this.name` en cada subclase — el nombre del constructor ya es el nombre correcto.

> [!tip]
> Añadir un campo `codigo` (string tipo `"NOT_FOUND"`) junto a `statusCode` facilita la internacionalización: el cliente puede mapear el código a mensajes en su propio idioma sin interpretar el texto de `message`.

> [!warning]
> `this.constructor.name` queda minificado en producción con bundlers como Terser. Si el `name` se usa en logging o monitoring, asignarlo explícitamente en cada subclase (`this.name = "ValidationError"`) es más robusto que depender del nombre del constructor.

## Notas relacionadas

- [[03 throw/01 Lanzar Errores | Lanzar Errores]]
- [[01 Tipos de Errores/index | Tipos de Errores nativos]]
- [[02 try - catch - finally/index | try / catch / finally]]
