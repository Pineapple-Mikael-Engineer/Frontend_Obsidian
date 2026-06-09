---
title: Step y Watch Expressions
aliases:
  - step debugger
  - watch expressions devtools
  - depuración paso a paso
tags:
  - javascript
  - api/concepto
  - errores
objeto: DevTools
tipo: herramienta
retorna: "-"
muta: false
asincrono: false
draft: false
---

# Step y Watch Expressions

> [!definicion]
> Una vez pausado en un breakpoint, el debugger de DevTools expone controles de paso (step) para avanzar la ejecución línea a línea o función a función, y paneles (Scope, Call Stack, Watch) para inspeccionar el estado completo del programa en ese punto exacto.

## Acciones de step

| Acción | Tecla (Chrome) | Comportamiento |
|--------|---------------|----------------|
| Resume | F8 | Continúa la ejecución hasta el próximo breakpoint o fin del script |
| Step Over | F10 | Ejecuta la línea actual completa sin entrar en las funciones que llama |
| Step Into | F11 | Entra en la primera función invocada en la línea actual |
| Step Out | Shift + F11 | Termina la función actual y vuelve al punto de llamada (caller) |
| Step | F9 | Avanza una instrucción — más granular que Step Over en expresiones complejas |

```
// Ejecución pausada en línea 5:
5 →  const resultado = calcular(a, procesarB(b));
```

- **Step Over (F10):** ejecuta la línea entera y para en la 6. No entra en `calcular` ni en `procesarB`.
- **Step Into (F11):** entra en `procesarB` (la primera función que se evalúa).
- **Step Out (Shift+F11):** desde dentro de `procesarB`, termina su ejecución y regresa a la línea 5.

## Scope panel

Muestra todas las variables accesibles en el punto de pausa, organizadas por ámbito:

| Sección | Qué contiene |
|---------|-------------|
| Local | Variables declaradas en la función actual |
| Closure | Variables de funciones externas capturadas por closures |
| Script | Variables de módulo / script al nivel superior |
| Global | Objeto `window` (browser) o `global` (Node) |

Los valores son **editables en vivo**: hacer doble clic en un valor en el Scope panel permite modificarlo sin recargar. Útil para probar hipótesis ("¿qué pasa si `x` fuera 0 aquí?") sin tocar el código.

## Call Stack panel

Muestra la cadena de llamadas que llevó al punto de pausa, de más reciente (arriba) a más antigua (abajo):

```
► calcularDescuento   [línea 12]
  aplicarPromocion    [línea 28]
  procesarCarrito     [línea 45]
  (anonymous)         [línea 3]
```

Hacer clic en cualquier frame del call stack salta a ese punto en el fuente y actualiza el Scope panel con las variables de ese frame. Permite inspeccionar el estado de cada función en la cadena de llamadas sin salir del debugger.

## Watch Expressions

Las Watch Expressions son expresiones JS evaluadas continuamente cada vez que el debugger pausa. Se añaden en el panel **Watch** con el botón `+`.

```js
// Expresiones de ejemplo en el panel Watch:
usuario.carrito.length
items.filter(i => i.activo).length
JSON.stringify(estado)
total * (1 - descuento)
```

A diferencia del Scope panel (que muestra variables tal cual), las Watch Expressions permiten monitorizar valores calculados, propiedades anidadas y cualquier expresión arbitraria. Se evalúan en el scope del frame activo en el Call Stack.

## Evaluate en consola durante pausa

Mientras el debugger está pausado, la pestaña **Console** tiene acceso completo al scope actual. Se puede ejecutar código arbitrario en el contexto del frame seleccionado:

```js
// Pausado dentro de procesarCarrito, en la consola:
> carrito.items.length      // → 3
> total.toFixed(2)          // → "149.99"
> items.find(i => i.id === 42)  // → { id: 42, nombre: "...", precio: ... }
> descuento = 0.2           // modifica la variable local en vivo
```

Es la herramienta más rápida para probar correcciones sin modificar el código: ajustar el valor de una variable, llamar una función con argumentos distintos y verificar el comportamiento.

## Receta: depurar un reducer

```js
// reducer.js
function reducer(estado, accion) {
  debugger; // pausa aquí
  switch (accion.type) {
    case "ADD_ITEM":
      return { ...estado, items: [...estado.items, accion.payload] };
    default:
      return estado;
  }
}
```

Flujo de depuración:
1. Pausar con `debugger` al entrar al reducer.
2. En el Scope panel: verificar `estado` (estado anterior) y `accion` (action object).
3. Añadir Watch Expression `estado.items.length` para ver cómo crece.
4. Step Over para ejecutar el `switch` y llegar al `return`.
5. En consola: `JSON.stringify(estado)` para capturar el estado antes de la mutación.
6. Step Out para volver al dispatcher y ver cómo se usa el valor retornado.

> [!tip]
> Activar **"Pause on caught exceptions"** en DevTools junto al uso de Step Into permite entrar en el bloque `catch` en el momento exacto en que se captura un error, con el objeto `err` ya disponible en el Scope panel — sin necesidad de añadir `console.log(err)` al código.

> [!warning]
> El Step Into (`F11`) puede llevar a código del motor JS o de librerías externas que no tiene source map. Usar **"Add script to ignore list"** (clic derecho en el archivo en Sources → "Add script to ignore list") para que DevTools salte automáticamente sobre ese código al hacer Step Into.

## Notas relacionadas

- [[04 Depuración/02 debugger y Breakpoints | debugger y Breakpoints]]
- [[04 Depuración/01 Métodos de console | Métodos de console]]
- [[02 try - catch - finally/index | try / catch / finally]]
