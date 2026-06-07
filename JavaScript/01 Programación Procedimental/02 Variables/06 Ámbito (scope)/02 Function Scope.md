---
title: Function Scope — El ámbito de función en JavaScript
aliases:
  - function scope
  - ámbito de función
  - local scope
tags:
  - javascript
  - api/concepto
  - variables
tipo: concepto
draft: false
---

# Function Scope

> [!definicion]
> El **ámbito de función** (*function scope*) es el ámbito creado por cada llamada a una función. Las variables declaradas dentro —incluidos los parámetros— existen solo durante esa llamada y no son accesibles desde el exterior. `var` respeta el ámbito de función aunque ignora el de bloque; `let` y `const` respetan ambos.

```js
function calcular() {
  var resultado = 42;    // solo accesible dentro de calcular()
  let auxiliar = 10;     // ídem
  return resultado + auxiliar;
}

calcular();
console.log(resultado); // ReferenceError: resultado is not defined
console.log(auxiliar);  // ReferenceError: auxiliar is not defined
```

## Aislamiento por llamada

Cada invocación de la función crea un **nuevo Environment Record** independiente. Los valores no persisten entre llamadas (salvo closures).

```js
function contarLlamadas() {
  var veces = 0; // se crea de nuevo en cada llamada
  veces++;
  console.log(veces);
}

contarLlamadas(); // 1
contarLlamadas(); // 1  ← nueva llamada, nuevo ámbito, nueva variable
contarLlamadas(); // 1
```

Para persistir el estado entre llamadas se usa un closure o una variable en un ámbito exterior.

## `var` respeta función pero no bloque

`var` declarada dentro de una función es local a esa función. Sin embargo, los bloques `if`/`for`/`while` dentro de la función **no crean un ámbito nuevo para `var`**: la variable queda visible en toda la función.

```js
function demostrar() {
  if (true) {
    var local = "filtrada";  // sube al ámbito de función
    let confinada = "bloque"; // solo en el if
  }

  console.log(local);     // "filtrada"  ← var se filtra del bloque
  console.log(confinada); // ReferenceError  ← let permanece en bloque
}
```

## Parámetros — ámbito de función implícito

Los parámetros de una función tienen ámbito de función: actúan como variables locales declaradas al inicio.

```js
function saludar(nombre, veces) {
  // nombre y veces son locales, como si fueran `var nombre; var veces;`
  console.log(`${nombre} × ${veces}`);
}

saludar("Ana", 3); // "Ana × 3"
console.log(nombre); // ReferenceError  ← nombre no existe aquí
```

Si un parámetro tiene el mismo nombre que una variable del ámbito exterior, el parámetro hace *shadowing* dentro de la función.

```js
const valor = "exterior";

function f(valor) {          // parámetro hace shadowing
  console.log(valor);        // el parámetro, no el exterior
}

f("interior"); // "interior"
console.log(valor); // "exterior"
```

## Funciones anidadas — base del closure

Una función declarada dentro de otra tiene acceso al ámbito de la función exterior. Este anidamiento es el mecanismo base de los closures.

```js
function exterior() {
  const secreto = "en exterior";

  function interior() {
    console.log(secreto); // accede al ámbito de exterior
  }

  interior(); // "en exterior"
}

exterior();
interior(); // ReferenceError: interior is not defined (no existe fuera)
```

La función `interior` puede leer y modificar variables de `exterior`, pero `exterior` no puede acceder a variables de `interior`. El acceso es unidireccional: hacia arriba en la cadena.

## Ejemplo práctico — encapsulación con función

Antes de los módulos ES6, el patrón IIFE usaba el ámbito de función para crear namespaces privados.

```js
const Contador = (function() {
  let _cuenta = 0; // privado — solo accesible a través de la API expuesta

  return {
    incrementar() { _cuenta++; },
    decrementar() { _cuenta--; },
    obtener()     { return _cuenta; }
  };
})();

Contador.incrementar();
Contador.incrementar();
console.log(Contador.obtener()); // 2
console.log(_cuenta);            // ReferenceError
```

## Cómo funciona por dentro

Al invocar una función, el motor crea un nuevo **Execution Context** y asocia un **Function Environment Record** a él. Este record almacena los bindings de parámetros y variables locales. El campo `[[OuterEnv]]` del record apunta al Environment Record del ámbito donde la función fue **definida** (no donde se llama), formando la scope chain. Cuando la llamada termina, el Execution Context se elimina de la pila y sus bindings se liberan (salvo que un closure mantenga una referencia al Environment Record).

> [!warning] Errores comunes
> **`var` que se filtra del bloque:** declarar `var` dentro de un `if` o `for` esperando que sea local al bloque. El ámbito real es la función completa. Usar `let` para confinamiento real al bloque.
> **Shadowing no intencional de parámetros:** nombrar una variable local igual que un parámetro crea confusión sobre cuál está en uso. El parámetro original pierde visibilidad en el sub-bloque.

> [!tip] Buenas prácticas
> Usar `let`/`const` para variables locales en lugar de `var`, ya que respetan también el bloque y producen errores descriptivos si se usan antes de declararse. Mantener las funciones pequeñas reduce el riesgo de shadowing accidental y facilita el razonamiento sobre el ámbito.

## Notas relacionadas

- [[index|Ámbito (scope)]]
- [[01 Global Scope]]
- [[03 Block Scope]]
- [[04 Lexical Scope]]
- [[../01 var|var]]
