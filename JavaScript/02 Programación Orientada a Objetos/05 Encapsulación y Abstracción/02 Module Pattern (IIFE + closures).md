---
title: Module Pattern (IIFE + closures)
aliases:
  - module pattern
  - revealing module pattern
  - IIFE module
  - patrón módulo javascript
tags:
  - javascript
  - api/concepto
  - encapsulacion
draft: false
order: 2
---

# Module Pattern (IIFE + closures)

> [!definicion]
> El Module Pattern combina una IIFE (Immediately Invoked Function Expression) con closures para crear un módulo singleton: estado privado encapsulado en el ámbito de la IIFE, API pública expuesta como el objeto que la IIFE devuelve. La IIFE se ejecuta exactamente una vez; el closure mantiene vivo su ámbito indefinidamente.

```js
const Contador = (function() {
  let _cuenta = 0;                                    // privada al módulo

  function _validar(n) { return typeof n === "number" && isFinite(n); }  // privada

  return {
    incrementar(n = 1) { if (_validar(n)) _cuenta += n; },
    reset()            { _cuenta = 0; },
    get valor()        { return _cuenta; }
  };
})();

Contador.incrementar(5);
Contador.valor;   // 5
Contador._cuenta; // undefined — inaccesible
```

La sintaxis `(function() { ... })()` envuelve la función en paréntesis para que el parser la interprete como expresión (no como declaración de función), y los `()` finales la invocan inmediatamente.

## Revealing Module Pattern

La variante más legible: todo se define como privado (variables y funciones con nombre), y al final se devuelve un objeto que "revela" explícitamente qué partes son públicas. La API pública está declarada en un solo lugar visible.

```js
const CarritoDeCompras = (function() {
  // --- estado privado ---
  const _items = [];

  // --- funciones privadas ---
  function _encontrar(id) {
    return _items.findIndex(i => i.id === id);
  }

  function _calcularTotal() {
    return _items.reduce((sum, i) => sum + i.precio * i.cantidad, 0);
  }

  // --- funciones públicas (aún privadas aquí) ---
  function agregar(producto) {
    const idx = _encontrar(producto.id);
    if (idx >= 0) _items[idx].cantidad++;
    else _items.push({ ...producto, cantidad: 1 });
  }

  function eliminar(id) {
    const idx = _encontrar(id);
    if (idx >= 0) _items.splice(idx, 1);
  }

  function listar() {
    return [..._items];  // copia defensiva — no expone el array interno
  }

  // --- API pública: revelada aquí ---
  return { agregar, eliminar, listar, get total() { return _calcularTotal(); } };
})();

CarritoDeCompras.agregar({ id: 1, nombre: "Libro", precio: 25 });
CarritoDeCompras.agregar({ id: 2, nombre: "Lápiz", precio: 3 });
CarritoDeCompras.total;   // 28
CarritoDeCompras.listar(); // [{id:1,...}, {id:2,...}]
```

La ventaja sobre el module pattern básico es la **separación visual** entre implementación (toda privada) y contrato (el `return`). Cualquier función se puede hacer pública simplemente añadiéndola al objeto devuelto, sin modificar su definición.

## Namespace pattern

Antes de los módulos ES y los bundlers, el problema principal era la contaminación del scope global. El Module Pattern resuelve esto agrupando todo bajo un único nombre global.

```js
// Sin módulos: riesgo de colisión
var utilidades = { ... };
var formatear = function() { ... };  // podría pisar otra variable global

// Con Module Pattern: un único nombre global
var MiApp = MiApp || {};  // namespace raíz

MiApp.Formateo = (function() {
  function _limpiar(str) { return str.trim().toLowerCase(); }
  function fecha(d)       { return d.toLocaleDateString("es-ES"); }
  function moneda(n)      { return `$${n.toFixed(2)}`; }
  return { fecha, moneda };
})();

MiApp.Formateo.fecha(new Date());    // "7/6/2026"
MiApp.Formateo.moneda(1234.5);       // "$1234.50"
```

## Augmentation (módulos ampliables)

El Module Pattern permite extender un módulo desde otro archivo, pasando el objeto como argumento de la IIFE.

```js
// archivo base
const Modulo = (function() {
  let _estado = 0;
  return { getEstado() { return _estado; } };
})();

// archivo de extensión
const Modulo = (function(mod) {
  mod.incrementar = function() { /* no puede acceder a _estado directamente */ };
  return mod;
})(Modulo || {});
```

La limitación: los módulos de extensión no pueden acceder al estado privado del módulo base — solo a la API pública devuelta. Para compartir estado privado entre archivos, es necesario pasarlo explícitamente o usar módulos ES.

## Por qué importa conocerlo

El Module Pattern es ubicuo en código JavaScript anterior a ES2015. Aparece en:
- jQuery y sus plugins (`(function($) { ... })(jQuery)`)
- Librerías UMD (Universal Module Definition)
- Código de aplicaciones web legacy sin bundler
- Scripts en entornos sin soporte de `import`/`export`

Leer código legacy requiere reconocer las variantes del patrón (con y sin argumentos, con namespace, con Revealing).

## Diferencia con módulos ES

| Aspecto | Module Pattern (IIFE) | ES Modules (`import`/`export`) |
|---|---|---|
| Evaluación | Al ejecutar la IIFE | Estático, en tiempo de carga |
| Scope privado | Ámbito léxico de la IIFE | Scope del archivo (top-level privado por defecto) |
| API pública | Objeto devuelto por la IIFE | Declaraciones `export` |
| Lazy loading | Manual | `import()` dinámico |
| Dependencias | Orden de `<script>` | Grafo resuelto por el motor |
| Tree shaking | Imposible (objeto opaco) | Posible (exportaciones estáticas) |

Los módulos ES son la forma canónica hoy. El Module Pattern es su emulación en entornos sin soporte nativo.

## Cómo funciona por dentro

La IIFE crea un nuevo ámbito de función en el call stack. Al finalizar, el frame se elimina del stack, pero el **environment record** del ámbito no se destruye porque el objeto devuelto (que vive en el ámbito externo) contiene métodos que lo referencian a través de sus slots `[[Environment]]`. El garbage collector mantiene ese environment record vivo mientras el objeto devuelto sea alcanzable. La IIFE en sí desaparece de memoria; lo que persiste es su ámbito capturado.

> [!tip] Buenas prácticas
> - Preferir el Revealing Module Pattern: la API pública declarada al final es un contrato explícito que facilita la lectura.
> - Pasar dependencias globales como argumentos de la IIFE (`(function($, _) { ... })(jQuery, lodash)`) en lugar de accederlas directamente — mejora la minificación y explicita las dependencias.
> - En proyectos modernos con bundler, preferir módulos ES. Reservar el Module Pattern para entornos legacy o scripts sin bundler.

> [!warning] Errores comunes
> - **Olvidar los paréntesis de invocación:** `(function() { ... })` sin los `()` finales solo define la función pero no la ejecuta — el módulo no se inicializa.
> - **Intentar acceder a estado privado desde extensiones:** los módulos de augmentation solo ven la API pública; el estado privado del módulo base es inaccesible por diseño.
> - **Singleton cuando se necesitan instancias:** el Module Pattern produce un único objeto. Si se necesitan múltiples instancias independientes, usar una [[01 Closures para Datos Privados | factory function con closures]] en su lugar.

## Notas relacionadas

- [[01 Closures para Datos Privados]] — el mecanismo léxico subyacente; factory functions para instancias múltiples
- [[03 Getters y Setters para Control de Acceso]] — getter `get valor()` dentro del objeto devuelto
- [[05 Encapsulación y Abstracción/index | Encapsulación]] — visión general de la subsección
