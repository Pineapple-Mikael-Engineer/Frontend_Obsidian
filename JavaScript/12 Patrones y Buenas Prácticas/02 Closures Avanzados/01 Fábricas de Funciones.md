---
title: Fábricas de Funciones — Funciones que crean funciones
aliases:
  - fábrica de funciones
  - function factory
  - factory function
tags:
  - javascript
  - api/concepto
  - patrones
draft: false
---

# Fábricas de Funciones

> [!definicion]
> Una **fábrica de funciones** es una función que retorna otras funciones configuradas con comportamiento o estado específico capturado en un closure. Cada llamada a la fábrica produce una función independiente con su propio entorno léxico.

```js
function crearMultiplicador(factor) {
  return (numero) => numero * factor;
}

const doble  = crearMultiplicador(2);
const triple = crearMultiplicador(3);

doble(5);   // → 10
triple(5);  // → 15
doble(triple(4)); // → 24
```

`factor` no es un argumento que se pase en cada uso — se fija una vez en la creación. La función retornada solo necesita el dato que varía por llamada.

## Casos de uso y recetas

### Validadores configurables

```js
function crearValidador(min, max) {
  return (valor) => {
    if (typeof valor !== 'number') throw new TypeError('Se esperaba un número');
    return valor >= min && valor <= max;
  };
}

const esEdadValida   = crearValidador(0, 120);
const esPorcentaje   = crearValidador(0, 100);
const esTempCorporal = crearValidador(35, 42);

esEdadValida(25);    // true
esPorcentaje(101);   // false
```

### Logger con prefijo y nivel

```js
function crearLogger(prefijo, nivel = 'log') {
  return (mensaje) => console[nivel](`[${prefijo}] ${mensaje}`);
}

const logAuth  = crearLogger('Auth', 'error');
const logApp   = crearLogger('App');
const logDebug = crearLogger('Debug', 'warn');

logAuth('Token inválido');   // [Auth] Token inválido  (en rojo)
logApp('Iniciado');          // [App] Iniciado
```

### Fetcher con base URL y headers fijos

```js
function crearApiClient(baseUrl, headers = {}) {
  return async (path, opciones = {}) => {
    const res = await fetch(`${baseUrl}${path}`, {
      headers: { 'Content-Type': 'application/json', ...headers },
      ...opciones
    });
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    return res.json();
  };
}

const api = crearApiClient('https://api.ejemplo.com', {
  Authorization: `Bearer ${TOKEN}`
});

const usuarios = await api('/usuarios');
const perfil   = await api('/usuarios/42');
```

### Handler de evento con contexto

```js
function crearHandlerSeleccion(lista, onSeleccion) {
  return (e) => {
    const item = e.target.closest('[data-id]');
    if (!item || !lista.contains(item)) return;
    onSeleccion(item.dataset.id, item);
  };
}

const handler = crearHandlerSeleccion(
  document.querySelector('#productos'),
  (id) => cargarDetalle(id)
);

document.querySelector('#productos').addEventListener('click', handler);
```

## Cómo funciona por dentro

Cada llamada a la fábrica ejecuta el cuerpo de la función externa y crea un scope nuevo. La función retornada se define dentro de ese scope y mantiene una referencia a él — ese es el closure. Cuando se invoca `doble(5)`, el motor busca `factor` en la cadena de scopes: no está en el scope local de la llamada a `doble`, pero sí en el scope de la ejecución de `crearMultiplicador(2)` que lo creó.

## Diferencia con currying

**Currying** es la transformación mecánica de `f(a, b)` en `f(a)(b)`: cada aplicación parcial retorna una nueva función que espera el siguiente argumento, hasta que todos estén satisfechos.

```js
// Currying estricto — f(a)(b)(c)
const sumar = (a) => (b) => a + b;
const sumar5 = sumar(5);
sumar5(3); // 8

// Fábrica — más general, puede configurar comportamiento, no solo parámetros
const crearSumador = (base) => (n, multiplicador = 1) => base + n * multiplicador;
const sumador5 = crearSumador(5);
sumador5(3);    // 8
sumador5(3, 2); // 11
```

Las fábricas son más generales: pueden capturar estado mutable, registrar listeners, mantener cachés, y retornar objetos complejos — no solo funciones de aridad reducida.

## Diferencia con clases

Una fábrica retorna un objeto sin `new`, sin `this`, sin herencia. La diferencia operativa:

| Aspecto | Fábrica de función | Clase |
|---|---|---|
| Invocación | `const x = crearX(config)` | `const x = new X(config)` |
| `this` | No necesario | Necesario; puede perderse |
| Herencia | No (composición explícita) | Sí, con `extends` |
| Campos privados | Closure (genuinamente privado) | `#campo` (sintaxis nativa) |
| Métodos compartidos | Cada instancia tiene copia | Compartidos en el prototipo |
| Uso adecuado | Módulos, configuración, callbacks | Jerarquías, muchas instancias |

Para casos donde no se necesita jerarquía ni muchas instancias, la fábrica es más simple y elimina los problemas de `this`.

> [!tip]
> Las fábricas se componen naturalmente: `crearApiClient` puede usarse dentro de `crearServicioUsuarios`, que a su vez se usa dentro de `crearStore`. Cada capa fija su configuración sin conocer la implementación de la siguiente.

> [!warning]
> Si se crean muchas instancias (miles), cada función retornada es un objeto nuevo en memoria — los métodos no se comparten por prototipo. Para clases con muchas instancias donde el rendimiento importa, las clases con prototipo son más eficientes.

## Notas relacionadas

- [[02 Datos Privados|Datos Privados]]
- [[index|Closures Avanzados — Índice]]
