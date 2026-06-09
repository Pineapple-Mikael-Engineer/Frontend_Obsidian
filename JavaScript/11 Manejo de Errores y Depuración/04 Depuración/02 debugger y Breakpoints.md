---
title: debugger y Breakpoints
aliases:
  - debugger statement
  - breakpoints devtools
  - puntos de interrupción javascript
tags:
  - javascript
  - api/concepto
  - errores
objeto: debugger
tipo: sentencia
retorna: "-"
muta: false
asincrono: false
draft: false
---

# debugger y Breakpoints

> [!definicion]
> La sentencia `debugger` pausa la ejecución del script cuando DevTools está abierto, en el punto exacto donde aparece. Es equivalente a colocar un breakpoint manual en el panel Sources, pero vive en el código fuente. Si DevTools está cerrado, `debugger` se ignora sin efecto.

## La sentencia debugger

```js
function calcularDescuento(precio, porcentaje) {
  debugger; // pausa aquí si DevTools está abierto
  return precio * (1 - porcentaje / 100);
}
```

Al pausar, DevTools muestra el estado completo del scope local, el call stack y permite avanzar paso a paso. Eliminar o comentar `debugger` antes de hacer commit — muchos linters (`eslint`) lo marcan como `warn` o `error` con la regla `no-debugger`.

## Tipos de breakpoints en Chrome DevTools

### Line-of-code breakpoints

Clic en el número de línea en el panel **Sources → (archivo)** → punto azul aparece en el margen. La ejecución pausa justo antes de ejecutar esa línea. Persisten mientras DevTools esté abierto; se pueden guardar con "Enable Local Overrides".

### Conditional breakpoints

Clic derecho en el número de línea → **Add conditional breakpoint** → introducir una expresión JS. La ejecución solo pausa cuando la condición evalúa a `true`.

```js
// Pausa solo en la iteración 500 de un bucle de 1000
// Condición: i === 500
for (let i = 0; i < 1000; i++) {
  procesarItem(items[i]);
}
```

Sin conditional breakpoints, habría que pulsar Resume 500 veces. Con la condición `i === 500`, DevTools pasa directamente a esa iteración.

### Logpoints

Clic derecho en el número de línea → **Add logpoint** → introducir una expresión. DevTools loguea el valor en consola sin pausar la ejecución. Alternativa a `console.log` que no requiere modificar el código fuente.

```
// Logpoint en línea 42:
"Usuario: " + usuario.nombre + " — rol: " + usuario.rol
```

### DOM breakpoints

Panel **Elements** → clic derecho en un nodo → **Break on**:

| Tipo | Cuándo pausa |
|------|-------------|
| Subtree modifications | Al añadir, eliminar o mover nodos dentro del nodo seleccionado |
| Attribute modifications | Al cambiar atributos del nodo seleccionado |
| Node removal | Al eliminar el nodo seleccionado del DOM |

Útil para rastrear qué código modifica el DOM sin saber de dónde proviene la mutación.

### XHR/Fetch breakpoints

Panel **Sources → XHR/fetch Breakpoints** → botón `+` → introducir una URL (o parte de ella). Pausa cuando se realiza una petición cuya URL contiene ese string. Permite interceptar peticiones sin modificar el código de `fetch` o `XMLHttpRequest`.

### Exception breakpoints

Panel **Sources** → icono de pausa con octágono → opciones:

| Opción | Comportamiento |
|--------|---------------|
| Pause on uncaught exceptions | Pausa solo en errores no capturados por ningún `catch` |
| Pause on caught exceptions | Pausa también dentro de bloques `catch` |

"Pause on caught exceptions" permite inspeccionar el error en el momento del `catch`, antes de que el handler lo procese.

### Event listener breakpoints

Panel **Sources → Event Listener Breakpoints** → expandir categorías (Mouse, Keyboard, XHR, Timer…) → activar el tipo de evento. Pausa cuando ese tipo de evento se dispara, independientemente de qué listener lo maneje.

```
// Activar: Keyboard → keydown
// → pausa cada vez que el usuario presiona una tecla
```

> [!tip]
> Los breakpoints de DevTools se pueden desactivar temporalmente (sin borrar) con el toggle "Deactivate breakpoints" (icono de breakpoint tachado en la barra de Sources). Útil para dejar los breakpoints configurados y ejecutar sin pausas mientras se reproduce el escenario.

> [!warning]
> Los breakpoints de line-of-code en código minificado o bundleado apuntan al bundle, no al fuente original. Para depurar correctamente código transpilado o bundleado, es necesario que el bundler genere **source maps** (`sourceMappingURL` al final del bundle). Con source maps activos, DevTools muestra el fuente original y los breakpoints se aplican sobre él.

## Notas relacionadas

- [[04 Depuración/01 Métodos de console | Métodos de console]]
- [[04 Depuración/03 Step y Watch Expressions | Step y Watch Expressions]]
- [[02 try - catch - finally/index | try / catch / finally]]
