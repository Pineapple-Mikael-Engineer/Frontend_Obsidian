---
title: Parámetros por Defecto — Valores cuando el argumento es undefined
aliases:
  - default parameters
  - parámetros por defecto
  - default values
tags:
  - javascript
  - api/concepto
  - funciones
objeto: global
tipo: concepto
draft: false
---

# Parámetros por Defecto

> [!definicion]
> Los **parámetros por defecto** permiten especificar un valor de sustitución para un parámetro cuando el argumento correspondiente es `undefined` (ausente o pasado explícitamente como `undefined`). El valor por defecto se evalúa en **cada llamada**, no una sola vez en la definición de la función.

```js
function conectar(host, puerto = 3000, protocolo = "https") {
  return `${protocolo}://${host}:${puerto}`;
}

conectar("api.ejemplo.com");               // → "https://api.ejemplo.com:3000"
conectar("api.ejemplo.com", 8080);         // → "https://api.ejemplo.com:8080"
conectar("api.ejemplo.com", 8080, "http"); // → "http://api.ejemplo.com:8080"
```

## Qué activa el default: solo `undefined`

`null`, `0`, `""` y `false` **no activan** el valor por defecto; son valores definidos. Solo `undefined` lo activa:

| Argumento pasado | Usa default |
|---|---|
| ausente | sí |
| `undefined` explícito | sí |
| `null` | no (`null` se recibe tal cual) |
| `0`, `""`, `false` | no |

```js
function saludo(nombre = "Visitante") {
  return `Hola, ${nombre}`;
}

saludo();            // → "Hola, Visitante"
saludo(undefined);   // → "Hola, Visitante"
saludo(null);        // → "Hola, null"  ← null NO activa el default
saludo("");          // → "Hola, "      ← string vacío NO activa el default
```

## Defaults con expresiones arbitrarias

El valor por defecto puede ser cualquier expresión válida: una llamada a función, un objeto, un array. Se evalúa en cada llamada, lo que es crítico para valores mutables:

```js
// CORRECTO: el array se crea nuevo en cada llamada
function acumular(valor, lista = []) {
  lista.push(valor);
  return lista;
}

acumular(1); // → [1]
acumular(2); // → [2]  ← lista nueva, no contaminada

// Comparar con el antipatrón en Python (mutable default):
// en JS esto NO es problema por la semántica de evaluación por llamada
```

```js
// Default que llama a otra función
function obtenerFecha() {
  return new Date().toISOString().slice(0, 10);
}

function registrar(evento, fecha = obtenerFecha()) {
  return `[${fecha}] ${evento}`;
}

registrar("login");          // → "[2026-06-07] login"
registrar("logout", "2026-01-01"); // → "[2026-01-01] logout"
```

## Referencia entre parámetros

Los defaults pueden referenciar parámetros anteriores (izquierda a derecha). La referencia inversa produce un TDZ:

```js
// OK — b se evalúa después de a
function rect(ancho, alto = ancho * 2) {
  return ancho * alto;
}
rect(5); // → 50  (alto = 10)

// ERROR — a intenta referenciar b que aún no existe
function invalido(a = b * 2, b = 5) {} // ReferenceError en la llamada
```

## Efecto en `fn.length`

Los parámetros con default no se cuentan en `fn.length`. La propiedad `length` refleja solo los parámetros requeridos (los que preceden al primer default):

```js
function f(a, b, c = 10, d = 20) {}
f.length; // → 2  (solo a y b cuentan)

function g(a = 1, b, c) {}
g.length; // → 0  (el primer default corta el conteo)
```

## Recetas comunes

**Opciones con objeto de configuración**:

```js
function crearElemento(tag, { clase = "", id = "", texto = "" } = {}) {
  const el = document.createElement(tag);
  if (clase) el.className = clase;
  if (id) el.id = id;
  if (texto) el.textContent = texto;
  return el;
}

crearElemento("div");                          // sin error aunque no se pasen opciones
crearElemento("p", { clase: "nota", texto: "Hola" });
```

**Validación con throw en el default**:

```js
function requerido(nombre) {
  throw new TypeError(`El parámetro "${nombre}" es requerido`);
}

function obtenerUsuario(id = requerido("id")) {
  return fetch(`/api/users/${id}`);
}

obtenerUsuario();     // → TypeError: El parámetro "id" es requerido
obtenerUsuario(42);   // OK
```

## Cómo funciona por dentro

El motor crea un **ámbito intermedio** (entre el scope léxico externo y el cuerpo de la función) donde se inicializan los parámetros. Cada parámetro es un binding en ese ámbito, lo que explica que los defaults puedan referenciar los anteriores (ya inicializados) pero no los siguientes (TDZ).

En pseudocódigo:

```
// Al llamar f(argA):
scope_parametros = new Scope(externo)
scope_parametros.a = argA !== undefined ? argA : <evaluar expresión default de a>
scope_parametros.b = argB !== undefined ? argB : <evaluar expresión default de b usando scope_parametros.a>
scope_cuerpo = new Scope(scope_parametros)
// ejecutar cuerpo en scope_cuerpo
```

> [!tip] Buenas prácticas
> - Poner los parámetros con default **al final** de la firma para preservar `fn.length` y mantener la API predecible.
> - No usar objetos o arrays mutables literalmente como default: aunque en JS se evalúan por llamada (no hay el bug de Python), es más legible construirlos explícitamente cuando hay lógica de inicialización.
> - El patrón `opcs = {}` es idiomático para funciones con múltiples opciones opcionales.

> [!warning] Errores comunes
> **Asumir que `null` activa el default** — el error más frecuente:
> ```js
> function f(x = 10) { return x; }
> f(null); // → null, NO → 10
> // Si la API puede devolver null y se quiere el default, usar: f(valor ?? undefined)
> ```
> **Default con efecto secundario no intencionado** — si el default llama a una función con efectos secundarios, se ejecuta en cada llamada donde el parámetro no se pase:
> ```js
> let llamadas = 0;
> function contadora() { return ++llamadas; }
>
> function f(n = contadora()) { return n; }
> f();  // llamadas = 1
> f();  // llamadas = 2
> f(0); // llamadas = 2  (el 0 no activa el default)
> ```

## Notas relacionadas

- [[07 Funciones (básico)/index | Funciones (básico)]]
- [[04 Rest Parameters]]
- [[05 Objeto arguments]]
- [[JavaScript/01 Programación Procedimental/02 Variables/05 Temporal Dead Zone | Temporal Dead Zone]]
