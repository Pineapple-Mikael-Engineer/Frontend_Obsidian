---
title: Closures para Datos Privados
aliases:
  - closure privacidad
  - factory function closure
  - datos privados closure
tags:
  - javascript
  - api/concepto
  - encapsulacion
draft: false
---

# Closures para Datos Privados

> [!definicion]
> Un closure es una función que captura y mantiene viva una referencia al ámbito léxico donde fue creada. Las variables de ese ámbito son inaccesibles desde el exterior — privacidad real sin ningún mecanismo especial del motor. El patrón de encapsulación consiste en: (1) declarar variables privadas dentro de una función, (2) definir funciones internas que las usen, y (3) devolver esas funciones como métodos de un objeto público.

```js
function crearContador(inicio = 0) {
  let cuenta = inicio;  // privada — solo visible en este ámbito léxico

  return {
    incrementar() { cuenta++; },
    decrementar() { cuenta--; },
    valor()       { return cuenta; }
  };
}

const c = crearContador(10);
c.incrementar();
c.valor();   // 11
c.cuenta;    // undefined — inaccesible
```

Cada método del objeto devuelto es un closure que cierra sobre `cuenta`. El motor mantiene el ámbito de `crearContador` vivo en el heap mientras alguno de esos métodos exista.

## Instancias independientes

Cada llamada a la función factory crea un nuevo ámbito léxico — y por tanto un nuevo conjunto de variables privadas. Las instancias son completamente independientes.

```js
const a = crearContador(0);
const b = crearContador(100);

a.incrementar();
b.decrementar();

a.valor(); // 1   — estado propio de a
b.valor(); // 99  — estado propio de b
```

Esto contrasta con los campos de clase ordinarios (públicos y compartidos a nivel de `prototype` para métodos, pero por instancia para propiedades de datos). Con closures, no existe ninguna propiedad pública que pueda mutarse desde fuera.

## Recetas comunes

### Múltiples variables privadas

```js
function crearPila() {
  const items = [];
  let tope = -1;

  return {
    push(val)  { items[++tope] = val; },
    pop()      { return tope < 0 ? undefined : items[tope--]; },
    peek()     { return items[tope]; },
    tamano()   { return tope + 1; },
    estaVacia(){ return tope < 0; }
  };
}

const pila = crearPila();
pila.push(1); pila.push(2);
pila.pop();   // 2
pila.tamano();// 1
```

### Variables privadas compartidas entre instancias

Para estado compartido entre todas las instancias (equivalente a campos estáticos), la variable se declara en el ámbito del módulo, fuera de la función factory.

```js
let _totalInstancias = 0;  // compartida entre todas las instancias

function crearUsuario(nombre) {
  _totalInstancias++;
  const _id = _totalInstancias;  // privada por instancia

  return {
    getNombre() { return nombre; },
    getId()     { return _id; },
    static_totalInstancias() { return _totalInstancias; }  // expuesto explícitamente
  };
}

const u1 = crearUsuario("Ana");
const u2 = crearUsuario("Luis");
u1.getId();  // 1
u2.getId();  // 2
u2.static_totalInstancias(); // 2
```

### Funciones privadas (helpers)

Las funciones internas no devueltas son igualmente privadas.

```js
function crearValidador() {
  function _esEmail(str) {          // privada — no expuesta
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(str);
  }
  function _esPositivo(n) {         // privada
    return typeof n === "number" && n > 0;
  }

  return {
    validarEmail(str)  { return _esEmail(str); },
    validarPrecio(n)   { return _esPositivo(n); }
  };
}
```

## Cómo funciona por dentro

Cuando el motor ejecuta `crearContador()`, crea un **environment record** (registro de entorno) en el heap que contiene `cuenta`. Cada función devuelta (`incrementar`, `decrementar`, `valor`) almacena internamente una referencia (`[[Environment]]`) a ese registro. Mientras alguna de esas funciones sea alcanzable, el garbage collector no puede liberar el registro.

El slot `[[Environment]]` es parte de la especificación de funciones (ECMAScript §10.2) y no es accesible desde JavaScript: no existe ninguna API que permita leer las variables capturadas de un closure desde el exterior.

Contraste con el stack: las variables locales de funciones normales viven en el stack de llamadas y se destruyen al retornar. Las variables de un closure viven en el heap porque su tiempo de vida excede al de la llamada que las creó.

## Closures vs campos privados `#`

| Aspecto | Closures | Campos `#` (ES2022) |
|---|---|---|
| Soporte del motor | No (convención léxica) | Sí (SyntaxError al acceder desde fuera) |
| Necesita `class` | No | Sí |
| Métodos en prototype | No (cada instancia tiene copia) | Sí (compartidos) |
| Herencia | Manual o imposible | Con `extends` |
| Coste de memoria | Mayor (métodos por instancia) | Menor (métodos en prototype) |
| Flexibilidad | Alta (sin estructura impuesta) | Media (requiere declaración en clase) |

El coste de memoria de los closures es relevante cuando se crean muchas instancias: cada instancia tiene sus propias copias de los métodos (funciones distintas), mientras que con clases todos comparten el mismo método en `prototype`.

> [!tip] Buenas prácticas
> - Usar el prefijo `_` para variables privadas en el ámbito del closure es opcional (son ya verdaderamente privadas), pero ayuda a la lectura del código dentro de la función.
> - Para módulos singleton (un solo objeto), preferir el [[02 Module Pattern (IIFE + closures) | Module Pattern]] en lugar de una factory function invocada una sola vez.
> - Cuando se necesiten muchas instancias con lógica compleja, considerar campos `#` de clase para evitar la duplicación de métodos en memoria.

> [!warning] Errores comunes
> - **Closure sobre variable de bucle:** capturar una variable de `var` en un bucle genera el problema clásico donde todos los closures capturan la misma variable (la solución es `let` o una IIFE por iteración).
>   ```js
>   // Incorrecto con var:
>   const fns = [];
>   for (var i = 0; i < 3; i++) {
>     fns.push(() => i); // todas capturan la misma `i`
>   }
>   fns[0](); // 3, no 0
>
>   // Correcto con let:
>   for (let i = 0; i < 3; i++) {
>     fns.push(() => i); // cada iteración tiene su propio `i`
>   }
>   fns[0](); // 0
>   ```
> - **Memory leak:** si los closures mantienen vivas referencias a estructuras grandes (arrays, nodos DOM), el ámbito no se libera. Asignar `null` a las variables cuando ya no se necesiten.
> - **this inesperado:** los métodos del objeto devuelto no son arrow functions; si se extraen y llaman sin receptor, `this` no será el objeto. Usar arrow functions o `bind` si el método necesita `this`.

## Notas relacionadas

- [[02 Module Pattern (IIFE + closures)]] — closures aplicados a módulos singleton con API revealing
- [[03 Getters y Setters para Control de Acceso]] — propiedades de acceso sobre variables de closure
- [[03 Clases/06 Campos Privados (#) | Campos Privados (#)]] — alternativa nativa del motor
- [[05 Encapsulación y Abstracción/index | Encapsulación]] — visión general de la subsección
