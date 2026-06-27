---
title: Métodos — funciones como propiedades de objeto
aliases:
  - object methods
  - method chaining
  - this en método
tags:
  - javascript
  - api/concepto
  - objetos
objeto: Object
tipo: concepto
muta: false
draft: false
order: 2
---

# Métodos

> [!definicion]
> Un método es una propiedad de un objeto cuyo valor es una función. Al invocar `obj.metodo()`, el motor vincula `this` al objeto receptor (el objeto a la izquierda del `.`), lo que permite que la función acceda y modifique el estado del propio objeto.

```js
const contador = {
  valor: 0,
  incrementar() {        // shorthand: equivale a incrementar: function() {}
    this.valor += 1;
    return this;         // devolver this habilita encadenamiento
  },
  resetear() {
    this.valor = 0;
    return this;
  },
};

contador.incrementar().incrementar().incrementar();
console.log(contador.valor); // → 3
contador.resetear();
console.log(contador.valor); // → 0
```

## Shorthand de método vs `function`

La sintaxis shorthand `{ saluda() {} }` y la forma larga `{ saluda: function() {} }` son funcionalmente equivalentes con una diferencia crítica: el shorthand habilita el uso de `super`, necesario para referirse al método en el prototipo padre. En objetos literales planos la diferencia rara vez importa, pero es determinante en objetos que forman parte de jerarquías prototípicas.

```js
const base = {
  tipo() { return "base"; },
};

const derivado = Object.create(base);
Object.assign(derivado, {
  tipo() {
    return super.tipo() + "+derivado"; // funciona solo con shorthand
  },
});

console.log(derivado.tipo()); // → "base+derivado"
```

## `this` — binding implícito

Cuando se invoca `obj.metodo()`, el motor establece `this = obj` para esa llamada. Este binding es dinámico: depende del sitio de llamada, no del sitio de definición.

```js
const reloj = {
  hora: "12:00",
  mostrar() { return this.hora; },
};

console.log(reloj.mostrar()); // → "12:00"

const fn = reloj.mostrar;     // se extrae la función del objeto
console.log(fn());            // → undefined (this = undefined en modo estricto)
                              // → "" (this = globalThis en modo no estricto)
```

La pérdida de `this` al extraer un método es la trampa más común. Se resuelve con `fn.bind(reloj)`, con una arrow function envolvente, o manteniendo siempre la llamada a través del objeto receptor. El tema completo se delega a [[04 this/index | this]].

## Method chaining

Devolver `this` al final de cada método permite encadenar llamadas sobre el mismo objeto. El patrón es análogo al de los builders en lenguajes tipados.

```js
const consulta = {
  _tabla: "",
  _condicion: "",
  _limite: null,

  from(tabla) { this._tabla = tabla; return this; },
  where(cond)  { this._condicion = cond; return this; },
  limit(n)     { this._limite = n; return this; },
  build() {
    return `SELECT * FROM ${this._tabla} WHERE ${this._condicion} LIMIT ${this._limite}`;
  },
};

const sql = consulta
  .from("usuarios")
  .where("activo = true")
  .limit(10)
  .build();

console.log(sql);
// → "SELECT * FROM usuarios WHERE activo = true LIMIT 10"
```

## `toString()` y `valueOf()` — coerción implícita

El motor llama a estos métodos cuando necesita convertir un objeto a primitivo (operación interna `ToPrimitive`). `valueOf` se usa en contextos numéricos; `toString` en contextos de string. Si ninguno devuelve un primitivo, se lanza `TypeError`.

```js
const temperatura = {
  grados: 100,
  unidad: "C",
  valueOf()  { return this.grados; },             // contexto numérico
  toString() { return `${this.grados}°${this.unidad}`; }, // contexto string
};

console.log(temperatura + 0);     // → 100   (se usa valueOf)
console.log(`${temperatura}`);    // → "100°C" (se usa toString en template literal)
console.log(temperatura > 50);    // → true  (se usa valueOf)
```

## Receta: objeto de configuración con API

```js
function crearPaginador({ porPagina = 10 } = {}) {
  return {
    _pagina: 1,
    _porPagina: porPagina,

    siguiente() { this._pagina += 1; return this; },
    anterior()  { this._pagina = Math.max(1, this._pagina - 1); return this; },
    ir(n)       { this._pagina = Math.max(1, n); return this; },
    estado()    { return { pagina: this._pagina, porPagina: this._porPagina }; },
  };
}

const p = crearPaginador({ porPagina: 20 });
console.log(p.ir(5).siguiente().estado()); // → { pagina: 6, porPagina: 20 }
```

## Cómo funciona por dentro

Cuando el motor ejecuta `obj.metodo()`, el proceso es:

1. Evalúa `obj.metodo` con `[[Get]]` — obtiene la función almacenada en la propiedad.
2. Usa la **referencia de propiedad** (no solo la función) para determinar el receptor: el objeto base de la referencia se convierte en `this`.
3. Llama a la función con `this` vinculado al receptor (binding implícito).

Este mecanismo es el **binding implícito de `this`**. Si la función se extrae antes de llamarla (`const fn = obj.metodo; fn()`), la referencia de propiedad se pierde y `this` cae al binding por defecto (`undefined` en modo estricto, `globalThis` en no estricto).

> [!tip] Buenas prácticas
> - Definir métodos con shorthand siempre que sea posible.
> - Devolver `this` en métodos de configuración para habilitar encadenamiento.
> - Sobreescribir `toString()` en objetos de dominio para depuración legible.

> [!warning] Errores comunes
> - Pasar `obj.metodo` como callback sin bindear: `setTimeout(obj.metodo, 1000)` pierde `this`. Usar `setTimeout(() => obj.metodo(), 1000)` o `setTimeout(obj.metodo.bind(obj), 1000)`.
> - Arrow functions como métodos: `{ fn: () => this.x }` captura el `this` del contexto de definición (probablemente `undefined` o `globalThis`), no el objeto receptor.

## Notas relacionadas

- [[04 this/index | this]] — todos los modos de binding de `this`
- [[05 Shorthand de Propiedades y Métodos]] — azúcar sintáctico de la definición de método
- [[06 Getters y Setters]] — propiedades de acceso como alternativa a métodos de lectura
- [[03 Arrow Functions/index | Arrow Functions]] — por qué no usan `this` propio
