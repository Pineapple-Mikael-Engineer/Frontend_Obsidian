---
title: Arrow Functions — Sin this propio (binding léxico)
aliases:
  - this léxico
  - lexical this
  - this en arrow functions
  - binding léxico this
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
---

# Sin this Propio

> [!definicion]
> Las arrow functions **no crean su propio `this`**. Cuando el motor evalúa `this` dentro de una arrow, sube por la cadena de scopes léxicos hasta encontrar el primer contexto de función (o módulo) que sí tenga `[[ThisValue]]` propio, y usa ese valor. El binding se fija en el momento de la **definición**, no en el de la invocación — por eso es léxico (estático), no dinámico.

```js
function Contador() {
  this.n = 0;

  // Función regular: this depende de quien la llame
  setInterval(function() {
    this.n++;             // this = window (o undefined en modo estricto)
  }, 1000);

  // Arrow: this capturado del constructor — siempre la instancia
  setInterval(() => {
    this.n++;             // this = instancia de Contador
  }, 1000);
}
```

## Binding léxico en la práctica

El caso de uso central de las arrows es el **callback dentro de un método de clase u objeto** donde se necesita acceder al `this` del método sin perderlo:

```js
class Timer {
  constructor() {
    this.segundos = 0;
  }

  iniciar() {
    // Arrow captura el this del método iniciar (la instancia)
    setInterval(() => {
      this.segundos++;
      console.log(this.segundos);
    }, 1000);
  }
}

const t = new Timer();
t.iniciar(); // 1, 2, 3, ...
```

Sin arrow, el callback de `setInterval` perdería el `this` de la instancia porque `setInterval` invoca la función sin receptor — el binding dinámico daría `window` o `undefined`.

## call, apply y bind no cambian el this de una arrow

En una función regular, `call(ctx)`, `apply(ctx)` y `bind(ctx)` reemplazan el `this` por `ctx`. En una arrow, el primer argumento de estos métodos se **ignora silenciosamente** — el binding léxico ya está fijado y no se puede sobreescribir. Los demás argumentos sí se pasan con normalidad:

```js
const obj  = { valor: 42 };
const flecha = () => this;

// El this léxico (module/window) se mantiene en todos los casos:
flecha.call(obj);    // → window o undefined (no obj)
flecha.apply(obj);   // → window o undefined
flecha.bind(obj)();  // → window o undefined
```

```js
// Los argumentos sí se pasan correctamente con call/apply:
const sumar = (a, b) => a + b;
sumar.call(null, 3, 4);   // → 7  — this ignorado, args sí usados
sumar.apply(null, [3, 4]); // → 7
```

## La trampa de la arrow como método de objeto literal

Si se define un método con arrow en un literal de objeto, `this` no apunta al objeto — apunta al scope donde se creó el literal (normalmente el módulo o `window`):

```js
const persona = {
  nombre: "Ana",

  // Función regular: this = persona
  saludarRegular() {
    return `Hola, soy ${this.nombre}`;
  },

  // Arrow: this = scope exterior al literal (módulo/global), NO persona
  saludarArrow: () => `Hola, soy ${this.nombre}`,
};

persona.saludarRegular(); // → "Hola, soy Ana"
persona.saludarArrow();   // → "Hola, soy undefined"
```

La arrow ve el scope del módulo en el momento en que se evalúa el literal de objeto — no ve al objeto `persona` porque el objeto no crea un nuevo contexto de `this`.

## Campos de clase arrow — ventajas e inconveniente

Definir un método como campo con arrow en una clase garantiza que `this` siempre sea la instancia, lo que resulta útil para handlers de eventos donde el receptor cambia:

```js
class Boton {
  constructor(texto) {
    this.texto = texto;
  }

  // Campo arrow: this siempre apunta a la instancia
  handleClick = () => {
    console.log(`Botón "${this.texto}" pulsado`);
  };
}

const btn = new Boton("Enviar");
document.addEventListener("click", btn.handleClick); // funciona sin .bind(btn)
```

**Inconveniente:** cada instancia crea su propia copia de `handleClick` en el heap, en lugar de compartir una única función en el `prototype`. Con miles de instancias, el coste de memoria puede ser relevante. Los métodos regulares en el prototype se comparten entre todas las instancias.

```js
// Método en prototype — compartido por todas las instancias
class BotonProto {
  handleClick() { console.log(this.texto); }
}

const a = new BotonProto();
const b = new BotonProto();
a.handleClick === b.handleClick; // → true — misma función en memoria

// Campo arrow — una copia por instancia
class BotonCampo {
  handleClick = () => console.log(this.texto);
}

const c = new BotonCampo();
const d = new BotonCampo();
c.handleClick === d.handleClick; // → false — dos funciones distintas
```

## Receta: resolver el problema de this en callbacks asíncronos

```js
class Servicio {
  constructor(url) {
    this.url = url;
    this.datos = null;
  }

  async cargar() {
    // Arrow en async: this sigue siendo la instancia durante toda la cadena
    const respuesta = await fetch(this.url);
    this.datos = await respuesta.json();
    return this.datos;
  }

  suscribir(elemento) {
    // Arrow en handler de evento: no hace falta bind
    elemento.addEventListener("click", () => {
      this.cargar().then(datos => console.log(datos));
    });
  }
}
```

## Cómo funciona por dentro

El spec de ECMAScript define las arrow functions como _Arrow Function Exotic Objects_ que carecen del slot interno `[[ThisValue]]`. Cuando el motor ejecuta `this` dentro de una arrow, dispara la operación `ResolveThisBinding()` que recorre el registro de entornos léxicos (`EnvironmentRecord`) hacia afuera hasta encontrar un `FunctionEnvironmentRecord` cuyo `[[ThisBindingStatus]]` no sea `"lexical"`. El primer `FunctionEnvironmentRecord` con binding propio es el que provee el `this`. Si la arrow está en el nivel de módulo, el `this` resultante es `undefined` (modo estricto implícito de los módulos ES); en un script clásico sin modo estricto sería `window`.

> [!tip] Buenas prácticas
> - Usa arrow para **callbacks dentro de métodos** donde necesitas heredar el `this` del método que los contiene.
> - Usa **campos de clase arrow** (`metodo = () => {}`) cuando el método se pasa como callback a APIs externas (eventos DOM, librerías de UI) y quieres `this` garantizado sin llamar a `bind`.
> - Para métodos de instancia normales que no se pasan como callbacks, prefiere métodos regulares en el prototype — son más eficientes en memoria y permiten `super`.

> [!warning] Errores comunes
> - Definir métodos de objeto literal con arrow: `this` no será el objeto, sino el scope exterior.
> - Esperar que `bind(nuevo)` cambie el `this` de una arrow: el primer argumento se ignora en silencio, sin error.
> - Confundir arrow en campo de clase con método de clase: ambos tienen `this` = instancia al invocarlos directamente sobre la instancia, pero el campo pierde el prototype sharing y no puede usar `super`.
> - En el nivel raíz de un módulo ES, el `this` léxico de una arrow es `undefined`, no `window`.

## Notas relacionadas

- [[01 Sintaxis y Return Implícito]] — sintaxis, parámetros y return implícito de las arrows
- [[03 Sin arguments ni new]] — otras limitaciones internas de las arrows
- [[04 Cuándo Usarlas]] — tabla de decisión completa para elegir entre arrow y función regular
