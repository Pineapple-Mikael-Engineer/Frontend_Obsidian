---
title: Lexical Scope — El ámbito léxico y la scope chain
aliases:
  - lexical scope
  - ámbito léxico
  - scope chain
  - cadena de ámbitos
tags:
  - javascript
  - api/concepto
  - variables
tipo: concepto
draft: false
---

# Lexical Scope

> [!definicion]
> El **ámbito léxico** (*lexical scope*) establece que el ámbito de una variable queda determinado por **dónde está escrita la función en el código fuente**, no desde dónde se llama en tiempo de ejecución. El motor resuelve los identificadores subiendo por la cadena de entornos anidados creada en el momento de la *definición* de cada función.

```js
const mensaje = "exterior";

function exterior() {
  const mensaje = "en exterior";

  function interior() {
    console.log(mensaje); // "en exterior"  ← toma del ámbito donde fue definida
  }

  return interior;
}

const fn = exterior();
fn(); // "en exterior"  ← aunque se llama desde el global, usa el ámbito léxico
```

## La scope chain

Cuando el motor busca un identificador, sigue la **cadena de ámbitos** hacia el exterior hasta encontrarlo o llegar al global:

```js
const a = "global";

function nivel1() {
  const b = "nivel1";

  function nivel2() {
    const c = "nivel2";

    function nivel3() {
      // Scope chain: nivel3 → nivel2 → nivel1 → global
      console.log(c); // "nivel2"   (propio)
      console.log(b); // "nivel1"   (nivel2 → nivel1)
      console.log(a); // "global"   (nivel2 → nivel1 → global)
      console.log(d); // ReferenceError  (no existe en ningún ámbito)
    }
    nivel3();
  }
  nivel2();
}
nivel1();
```

La búsqueda es **unidireccional**: hacia afuera (del ámbito local al global), nunca hacia adentro ni hacia los lados.

## Léxico vs. dinámico — la diferencia clave

En JavaScript el ámbito es **léxico** (estático): importa *dónde se define* la función. Un ámbito dinámico (que JS no tiene) haría que importara *desde dónde se llama*.

```js
const x = "global";

function leerX() {
  console.log(x); // ¿cuál x?
}

function contenedor() {
  const x = "local de contenedor";
  leerX(); // llama a leerX desde dentro de contenedor
}

contenedor(); // "global"  ← ámbito léxico: leerX ve el x de donde fue definida (global)
              //             ámbito dinámico (hipotético): vería "local de contenedor"
```

`leerX` fue definida en el ámbito global, por lo que su `[[OuterEnv]]` apunta al global. Eso no cambia independientemente de desde dónde se la llame.

## Lexical scope como base de los closures

Una función "recuerda" el Environment Record del ámbito donde fue definida aunque ese ámbito ya haya terminado de ejecutarse. Esa referencia persistente es un **closure**.

```js
function crearContador(inicio) {
  let cuenta = inicio;

  return {
    incrementar() { cuenta++; },
    obtener()     { return cuenta; }
  };
}

const c1 = crearContador(0);
const c2 = crearContador(100);

c1.incrementar();
c1.incrementar();
console.log(c1.obtener()); // 2
console.log(c2.obtener()); // 100  ← ámbito independiente
```

`c1` y `c2` son closures sobre Environment Records distintos: cada llamada a `crearContador` crea su propio entorno con su propia `cuenta`.

## Shadowing — ocultar una variable del ámbito exterior

Declarar una variable con el mismo nombre en un ámbito interior *oculta* (*shadow*) la variable del ámbito exterior sin modificarla.

```js
const color = "rojo";

function pintar() {
  const color = "azul"; // shadowing
  console.log(color);   // "azul"
}

pintar();
console.log(color); // "rojo"  ← la exterior no cambió
```

El shadowing es legítimo cuando el nombre en el ámbito interior tiene un rol diferente. Se convierte en bug cuando es accidental: el programador cree estar leyendo la variable exterior pero lee la interior.

## Ejemplo práctico — factory de funciones

El ámbito léxico permite crear funciones especializadas con estado capturado.

```js
function multiplicadorPor(factor) {
  return (numero) => numero * factor; // factor capturado del ámbito léxico
}

const doble = multiplicadorPor(2);
const triple = multiplicadorPor(3);

console.log(doble(5));  // 10
console.log(triple(5)); // 15
```

Cada arrow function captura su propio `factor` del Environment Record de la llamada correspondiente a `multiplicadorPor`.

## Cómo funciona por dentro

Cada función almacena internamente una referencia `[[Environment]]` al *Lexical Environment* donde fue **creada** (no donde se invoca). Al llamar a la función, el motor crea un nuevo entorno de llamada cuyo `[[OuterEnv]]` se fija en ese `[[Environment]]`. Esto construye la scope chain en tiempo de definición, no en tiempo de ejecución.

La especificación ECMAScript denomina este mecanismo *static scope* o *lexical scope* y lo implementa mediante la propiedad interna `[[Environment]]` de los objetos función.

```
Creación de multiplicadorPor(2):
  Environment Record { factor: 2 }
    ↑ [[OuterEnv]] → global

Creación de la arrow function (numero) => numero * factor:
  [[Environment]] → apunta a { factor: 2 }

Llamada doble(5):
  Nuevo ER { numero: 5 }
    ↑ [[OuterEnv]] → { factor: 2 }
  Búsqueda de factor: no en local → sube → { factor: 2 } → 2 ✓
```

> [!warning] Errores comunes
> **Confundir ámbito léxico con ámbito dinámico:** en JS, el lugar de definición manda, no el lugar de llamada. Asumir que una función "hereda" el contexto del llamador lleva a bugs sutiles con callbacks y métodos de objeto.
> **Shadowing accidental en funciones anidadas:** si una función interior declara una variable con el mismo nombre que una de la exterior, la exterior deja de ser visible en la interior sin ningún aviso del runtime.

> [!tip] Buenas prácticas
> El ámbito léxico hace que el comportamiento de las funciones sea predecible y fácil de razonar: solo hay que leer el código fuente para saber qué variables ve cada función. Aprovechar el closure para encapsular estado en lugar de variables globales. Evitar el shadowing cuando no es intencional: usar nombres descriptivos que no colisionen.

## Notas relacionadas

- [[index|Ámbito (scope)]]
- [[01 Global Scope]]
- [[02 Function Scope]]
- [[03 Block Scope]]
- [[../04 Hoisting|Hoisting]]
