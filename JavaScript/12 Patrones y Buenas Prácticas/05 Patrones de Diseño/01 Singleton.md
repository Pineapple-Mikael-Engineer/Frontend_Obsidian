---
title: Singleton
aliases:
  - patrón singleton
  - instancia única
tags:
  - javascript
  - api/concepto
  - patrones
draft: false
---

# Singleton

> [!definicion]
> El **Singleton** es un patrón de diseño que garantiza que una clase o módulo tenga **una única instancia** durante toda la vida de la aplicación, y proporciona un punto de acceso global a ella. En JavaScript, los módulos ES6 son singletons por naturaleza: el motor solo ejecuta el cuerpo del módulo una vez, independientemente de cuántos archivos lo importen.

## Implementación con módulos ES6 (recomendada)

La forma más sencilla y moderna: exportar un objeto literal o una instancia desde un módulo. El sistema de módulos garantiza que el archivo se inicialice exactamente una vez.

```js
// config.js
const config = {
  apiUrl: "https://api.ejemplo.com",
  timeout: 5000,
  maxReintentos: 3,
};
export default config;

// En cualquier archivo que lo importe:
// import config from "./config.js";
// config === la misma referencia siempre — no hay segunda instancia
```

```js
// logger.js
const logger = {
  _prefijo: "[App]",
  info(msg)  { console.log(`${this._prefijo} INFO:`, msg); },
  error(msg) { console.error(`${this._prefijo} ERROR:`, msg); },
};
export default logger;
```

## Implementación con clase (instanciación diferida)

Útil cuando la construcción de la instancia tiene un costo que se quiere diferir al primer uso, o cuando se necesita una API orientada a objetos.

```js
class Database {
  static #instancia = null;
  #conexion;

  constructor() {
    if (Database.#instancia) return Database.#instancia;
    this.#conexion = this.#crearConexion();
    Database.#instancia = this;
  }

  #crearConexion() {
    return { host: "localhost", puerto: 5432, abierta: true };
  }

  static getInstance() {
    return Database.#instancia ?? new Database();
  }

  query(sql) {
    if (!this.#conexion.abierta) throw new Error("Conexión cerrada");
    // ...ejecutar consulta
  }
}

const db1 = Database.getInstance();
const db2 = Database.getInstance();
// db1 === db2 → true
```

Los campos privados estáticos (`static #instancia`) impiden que el código externo sobrescriba el slot de instancia.

## Cuándo usar

| Caso de uso | Por qué singleton |
|---|---|
| Configuración global | Un único objeto de configuración evita desincronización |
| Conexión a base de datos | Costoso crear más de una; mejor reutilizar |
| Logger centralizado | Todos los módulos escriben al mismo destino |
| Store de estado (sin framework) | La fuente de verdad debe ser única |
| Caché compartida | Una sola caché; varias instancias duplicarían datos |

## Cómo funciona por dentro

Los módulos ES6 se cargan y ejecutan en un **module registry** del motor JS. La primera vez que un módulo se importa, el motor lo evalúa y almacena el resultado en el registro. Las importaciones posteriores del mismo especificador reciben el objeto ya evaluado, sin re-ejecución. Esto hace que cualquier objeto exportado sea, por definición, un singleton dentro de la misma ejecución del programa.

La implementación con clase replica este comportamiento manualmente: el campo estático `#instancia` persiste en la clase (no en ningún objeto en particular), de modo que cualquier llamada a `new Database()` que encuentre `#instancia` ya inicializado devuelve la instancia existente en lugar de crear una nueva.

> [!tip]
> En proyectos con módulos ES6 o un bundler, el patrón de exportar un objeto literal es preferible a la implementación con clase: es más simple, no requiere boilerplate, y el comportamiento de singleton está garantizado por la especificación del lenguaje.

> [!warning]
> El singleton dificulta el **testing aislado**: la instancia persiste entre tests y puede llevar estado residual de uno al siguiente. Dos estrategias de mitigación: (1) exponer un método `reset()` marcado como `@internal` o solo disponible en entorno de test, o (2) preferir **inyección de dependencias** — pasar la instancia compartida como parámetro en lugar de acceder a ella globalmente. El antipatrón más común es usar singleton para todo: introduce acoplamiento global y convierte cualquier pieza de estado en implícito.

## Notas relacionadas

- [[02 Observer (Pub-Sub)|Observer (Pub-Sub)]]
- [[05 Patrones de Diseño/index|Patrones de Diseño]]
