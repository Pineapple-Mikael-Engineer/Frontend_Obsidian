---
title: Pérdida de this — Detaching y soluciones
aliases:
  - pérdida de this
  - detaching
  - this perdido
  - this desvinculado
tags:
  - javascript
  - api/concepto
  - this
objeto: global
tipo: concepto
muta: false
asincrono: false
draft: false
order: 6
---

# Pérdida de `this` — Detaching y soluciones

> [!definicion]
> La **pérdida de `this`** ocurre cuando un método se extrae de su objeto receptor y se invoca sin él. El binding implícito del método con su objeto se rompe en el momento de la extracción: la función se convierte en una referencia sin base, y el motor aplica default binding (`undefined` en modo estricto, objeto global en no estricto).

```js
const reproductor = {
  pista: "Bohemian Rhapsody",
  reproducir() {
    console.log(`Reproduciendo: ${this.pista}`);
  }
};

reproductor.reproducir();        // → "Reproduciendo: Bohemian Rhapsody"

const fn = reproductor.reproducir;
fn();                            // → "Reproduciendo: undefined"  (strict: TypeError)
```

## Casos donde ocurre

### Asignación a variable

```js
const metodo = obj.metodo;
metodo(); // this = undefined / window
```

### Callback en setTimeout / setInterval

```js
class Contador {
  constructor() { this.n = 0; }
  tick() { this.n++; console.log(this.n); }
}

const c = new Contador();
setTimeout(c.tick, 500);  // → NaN o TypeError — this no es c
```

### Callback en addEventListener

```js
class Boton {
  constructor(label) { this.label = label; }
  clickHandler() { console.log(this.label); }
}

const b = new Boton("Enviar");
document.getElementById("btn").addEventListener("click", b.clickHandler);
// → undefined — this es el elemento DOM, no b
```

### Desestructuración de métodos

```js
const { reproducir } = reproductor;
reproducir(); // → this perdido
```

### Pasado a Array.prototype.forEach / map sin contexto

```js
[1, 2, 3].forEach(obj.procesar); // this en procesar = undefined
```

## Soluciones

### 1. Arrow function envolvente (inline)

Ideal para callbacks de un solo uso. La arrow captura el `this` del scope donde está definida.

```js
setTimeout(() => c.tick(), 500);          // → 1, 2, 3 …
btn.addEventListener("click", () => b.clickHandler());
```

### 2. `bind` — binding permanente para reutilizar

Ideal cuando el callback se pasa a múltiples sitios o se necesita poder removerlo.

```js
const tickAtado = c.tick.bind(c);
setTimeout(tickAtado, 500);               // → 1

// También puede registrarse y desregistrarse
btn.addEventListener("click", tickAtado);
btn.removeEventListener("click", tickAtado); // funciona — misma referencia
```

### 3. Campo de clase con arrow (patrón moderno)

La arrow se crea una vez por instancia en el constructor. `this` es siempre correcto y la referencia es estable.

```js
class Boton {
  constructor(label) {
    this.label = label;
  }

  // Campo de clase: se crea en el constructor, captura this de la instancia
  clickHandler = () => {
    console.log(this.label);
  };
}

const b = new Boton("Enviar");
btn.addEventListener("click", b.clickHandler);    // → "Enviar"
btn.removeEventListener("click", b.clickHandler); // → funciona
```

### 4. `bind` en el constructor (patrón pre-campos)

Equivalente al campo de clase con arrow, pero explícito en el constructor.

```js
class Formulario {
  constructor() {
    this.datos = {};
    this.handleSubmit = this.handleSubmit.bind(this);
    this.handleChange = this.handleChange.bind(this);
  }

  handleSubmit(e) { /* this es siempre la instancia */ }
  handleChange(e) { /* this es siempre la instancia */ }
}
```

## Cuándo usar cada solución

| Situación | Solución recomendada |
|---|---|
| Callback de un solo uso, no necesita ser removido | Arrow inline `() => obj.metodo()` |
| Callback reutilizable o que debe poder removerse | `bind` en declaración |
| Handlers de una clase que siempre necesitan el contexto correcto | Campo de clase con arrow |
| Código pre-ES2022 sin soporte de campos de clase | `bind` en el constructor |

## Cómo funciona por dentro

> [!info]
> El binding implícito depende de que la llamada incluya una **Reference con base de objeto**. Al extraer la función (`const fn = obj.metodo`), lo que se almacena en `fn` es solo el valor de la función — la Reference original con su base se descarta. Cuando `fn()` se evalúa, la Expression produce una Reference sin base, y se aplica default binding. El objeto `obj` y la función `fn` ya no están conectados en ninguna estructura del runtime.

## Buenas prácticas

> [!tip]
> En proyectos con clases y event listeners frecuentes, preferir campos de clase con arrow sobre `bind` en el constructor: más legible, menos propenso a olvidar un handler. Reservar el `bind` explícito para APIs que requieren partial application o para código que no puede usar campos de clase.

## Errores comunes

> [!warning]
> **Olvidar el `bind` en el constructor para un handler** es uno de los bugs más frecuentes en React antes de los hooks. El síntoma es `TypeError: Cannot read properties of undefined (reading 'setState')` dentro del handler. La regla simple: si un método se pasa como argumento (sin invocarse en ese momento), necesita estar atado a su objeto.

## Notas relacionadas

- [[index|this — Contexto de invocación]] — árbol de decisión
- [[02 this en Función]] — default binding, lo que ocurre tras la pérdida
- [[05 this en Arrow Functions]] — el binding léxico como alternativa estructural
- [[07 call()]] — explicit binding puntual
- [[09 bind()]] — detalles de la función exótica creada por bind
