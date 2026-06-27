---
title: Shorthand de Propiedades y Métodos — azúcar sintáctico ES6
aliases:
  - shorthand properties
  - shorthand methods
  - property shorthand
  - method shorthand
tags:
  - javascript
  - api/concepto
  - objetos
objeto: Object
tipo: concepto
draft: false
order: 5
---

# Shorthand de Propiedades y Métodos

> [!definicion]
> El shorthand de ES6 permite dos abreviaciones en literales de objeto: (1) **propiedad shorthand**: cuando el nombre de la variable y la clave coinciden, se puede escribir solo el identificador en lugar de `clave: variable`; (2) **método shorthand**: `{ fn() {} }` en lugar de `{ fn: function() {} }`. Son azúcar sintáctico puro; el parser los transforma a la forma larga equivalente. El shorthand de método habilita además el uso de `super`.

```js
const x = 10, y = 20;

// Forma larga
const punto1 = { x: x, y: y, distancia: function() { return Math.hypot(this.x, this.y); } };

// Shorthand equivalente
const punto2 = { x, y, distancia() { return Math.hypot(this.x, this.y); } };

console.log(punto2.distancia()); // → 22.36...
```

## Shorthand de propiedad

Si la clave y la variable tienen el mismo nombre, basta escribir el identificador:

```js
const nombre = "Ana";
const edad   = 30;
const activo = true;

const usuario = { nombre, edad, activo };
// equivale exactamente a:
// const usuario = { nombre: nombre, edad: edad, activo: activo };

console.log(usuario); // → { nombre: "Ana", edad: 30, activo: true }
```

### Receta: función que devuelve múltiples valores

```js
function calcularEstadisticas(datos) {
  const media    = datos.reduce((s, x) => s + x, 0) / datos.length;
  const varianza = datos.reduce((s, x) => s + (x - media) ** 2, 0) / datos.length;
  const desvEst  = Math.sqrt(varianza);
  return { media, varianza, desvEst }; // shorthand: clave = nombre de variable
}

const { media, desvEst } = calcularEstadisticas([2, 4, 4, 4, 5, 5, 7, 9]);
console.log(media);   // → 5
console.log(desvEst); // → 2
```

### Receta: objeto de respuesta estandarizado

```js
function procesarPago(monto, moneda) {
  const exito = monto > 0;
  const timestamp = Date.now();
  const referencia = `REF-${timestamp}`;
  return { exito, monto, moneda, timestamp, referencia };
}
```

## Shorthand de método

```js
const pila = {
  _items: [],
  push(item)  { this._items.push(item); return this; },
  pop()       { return this._items.pop(); },
  peek()      { return this._items.at(-1); },
  get size()  { return this._items.length; }, // getter (también shorthand)
};

pila.push(1).push(2).push(3);
console.log(pila.peek()); // → 3
console.log(pila.size);   // → 3
```

### Por qué el shorthand de método habilita `super`

En un objeto definido con shorthand, el motor establece el slot interno `[[HomeObject]]` apuntando al objeto que contiene el método. `super` usa ese slot para navegar por la cadena de prototipos. La forma larga `fn: function() {}` no establece `[[HomeObject]]`, por lo que `super` dentro de ella lanzaría un `ReferenceError`.

```js
const animal = {
  tipo() { return "Animal"; },
};

const perro = Object.create(animal);
Object.assign(perro, {
  tipo() {
    return super.tipo() + " → Perro"; // funciona: shorthand tiene [[HomeObject]]
  },
});
console.log(perro.tipo()); // → "Animal → Perro"
```

## Combinaciones prácticas

### Construir objeto de estado desde variables locales

```js
function crearSesion(token, userId, rol) {
  const expiracion = new Date(Date.now() + 3_600_000);
  const activo = true;
  return { token, userId, rol, expiracion, activo };
}
```

### Merge de objetos con shorthand

```js
const base = { tema: "oscuro", idioma: "es" };
const usuario = { nombre: "Ana" };

const config = { ...base, ...usuario }; // spread, no shorthand, pero idioma relacionado
// shorthand en el objeto local:
const tema = "claro";
const resultado = { ...base, tema }; // sobreescribe: { tema: "claro", idioma: "es" }
```

## Cómo funciona por dentro

Ambos shorthands son transformaciones puramente sintácticas realizadas por el parser. En el AST (árbol de sintaxis abstracta), `{ x }` produce exactamente el mismo nodo que `{ x: x }`, y `{ fn() {} }` produce el mismo nodo que `{ fn: function fn() {} }` — con la excepción de que el shorthand de método marca la función con `[[HomeObject]]`. No hay diferencia de rendimiento ni de comportamiento en tiempo de ejecución para el caso de propiedad; para el método, la única diferencia observable es la habilitación de `super`.

> [!tip] Buenas prácticas
> - Usar siempre shorthand de propiedad cuando el nombre de variable coincide con la clave: reduce ruido visual y evita duplicaciones.
> - Preferir shorthand de método sobre la forma larga para mantener la posibilidad de usar `super` en el futuro si el objeto se integra en una jerarquía prototípica.
> - Agrupar las propiedades shorthand al inicio del literal para que sean fáciles de identificar visualmente.

> [!warning] Errores comunes
> - Confundir shorthand de propiedad con destructuring: `const { x, y } = punto` (destructuring — extrae del objeto); `const nuevo = { x, y }` (shorthand — crea objeto). La sintaxis es similar pero la dirección es opuesta.
> - Usar la forma larga de función como método cuando se necesita `super`: fallará en herencia prototípica con `Object.create`.
> - En modo estricto (o TypeScript), intentar acceder a `super` en una función anónima o una arrow asignada como propiedad lanza `SyntaxError` o `ReferenceError`.

## Notas relacionadas

- [[04 Propiedades Calculadas]] — claves dinámicas, el otro tipo de azúcar sintáctico en literales
- [[02 Métodos]] — comportamiento de `this` en métodos
- [[02 Prototipos/index | Prototipos]] — `[[HomeObject]]` y la cadena de prototipos
- [[03 Clases/index | Clases]] — el shorthand de método es idéntico en clases
