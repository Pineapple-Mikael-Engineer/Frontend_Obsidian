---
title: this en Método — Implicit binding
aliases:
  - this en método
  - implicit binding
  - this con receptor
tags:
  - javascript
  - api/concepto
  - this
objeto: global
tipo: concepto
muta: false
asincrono: false
draft: false
---

# `this` en Método — Implicit binding

> [!definicion]
> Cuando una función se invoca con un receptor explícito — `obj.metodo()` — se aplica el **implicit binding**: `this` dentro del cuerpo de la función es el objeto a la izquierda del punto (o de los corchetes) en el momento de la llamada.

```js
const cuenta = {
  saldo: 1000,
  depositar(cantidad) {
    this.saldo += cantidad;
    return this.saldo;
  }
};

cuenta.depositar(500); // → 1500  — this === cuenta
```

## El receptor es dinámico: depende del sitio de llamada

La misma función puede tener distintos `this` según cómo se invoque. El binding no se fija en la definición.

```js
function saludar() {
  return `Hola, soy ${this.nombre}`;
}

const a = { nombre: "Ana", saludar };
const b = { nombre: "Bea", saludar };

a.saludar(); // → "Hola, soy Ana"
b.saludar(); // → "Hola, soy Bea"
```

## Receptor con acceso por corchetes

El binding se aplica igualmente cuando el método se accede con notación de corchetes.

```js
const llave = "depositar";
cuenta[llave](200); // → this === cuenta  — mismo binding que con punto
```

## Encadenamiento de métodos y `this`

Si un método devuelve `this`, los métodos encadenados reciben el mismo objeto como receptor.

```js
class Consulta {
  constructor() {
    this._filtros = [];
    this._limite = null;
  }

  donde(condicion) {
    this._filtros.push(condicion);
    return this;          // permite encadenar
  }

  limite(n) {
    this._limite = n;
    return this;
  }

  ejecutar() {
    return `Filtros: ${this._filtros.join(", ")} | Límite: ${this._limite}`;
  }
}

new Consulta()
  .donde("activo = true")
  .donde("rol = admin")
  .limite(10)
  .ejecutar();
// → "Filtros: activo = true, rol = admin | Límite: 10"
```

## Acceso a propiedades heredadas desde `this`

`this.prop` busca primero en el propio objeto y luego sube por la cadena de prototipos. Un método de prototipo accede a las propiedades de instancia a través de `this`.

```js
function Animal(nombre) {
  this.nombre = nombre;
}

Animal.prototype.presentarse = function () {
  return `Soy ${this.nombre}`;   // this = instancia concreta en llamada
};

const perro = new Animal("Rex");
perro.presentarse(); // → "Soy Rex"
```

## Cómo funciona por dentro

> [!info]
> Al evaluar `obj.metodo()`, el motor genera una **Reference** con base `obj` y nombre `"metodo"`. Antes de crear el frame de ejecución de la función, lee la base de la Reference y la establece como `ThisValue`. La función en sí no contiene información sobre su receptor — todo el binding ocurre en el call-site. Si la Reference no tiene base (función extraída de su objeto), el binding se pierde y cae al default.

## Buenas prácticas

> [!tip]
> Al leer código con `this`, siempre identificar **el objeto a la izquierda del punto en la línea de llamada**, no donde está definida la función. Si la función se llama como callback o se asigna a variable, el binding ya no es implícito — ver [[06 Pérdida de this]].

## Errores comunes

> [!warning]
> **Doble anidamiento de objetos**: solo el objeto inmediatamente a la izquierda del punto cuenta. En `a.b.metodo()`, `this === b`, no `a`. Si `b` no tiene la propiedad que se busca en `this`, el acceso devuelve `undefined` en lugar del error esperado (especialmente confuso con modo no estricto).

```js
const externo = {
  nombre: "externo",
  interno: {
    nombre: "interno",
    quien() { return this.nombre; }
  }
};

externo.interno.quien(); // → "interno"  — this = externo.interno, no externo
```

## Notas relacionadas

- [[index|this — Contexto de invocación]] — los 5 contextos y prioridades
- [[06 Pérdida de this]] — qué ocurre al extraer el método del objeto
- [[02 Prototipos/index|Prototipos]] — cómo `this` atraviesa la cadena prototípica
- [[03 Clases/index|Clases]] — métodos de instancia y `this` en clases
