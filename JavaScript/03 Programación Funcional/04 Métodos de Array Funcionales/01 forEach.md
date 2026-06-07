---
title: "forEach"
aliases:
  - Array forEach
  - array.forEach
tags:
  - javascript
  - api/metodo
  - arrays
draft: false
---

# forEach

> [!definicion]
> `Array.prototype.forEach` ejecuta una función callback una vez por cada elemento del array, en orden de índice ascendente. Devuelve siempre `undefined` y no se puede interrumpir anticipadamente. Su propósito es producir **efectos secundarios**, no transformar datos.

```js
const frutas = ["manzana", "banana", "cereza"];

frutas.forEach((fruta, i) => {
  console.log(`${i}: ${fruta}`);
});
// 0: manzana
// 1: banana
// 2: cereza
// Retorno: undefined
```

## Firma del método

| Parámetro del callback | Tipo | Descripción |
|---|---|---|
| `elemento` | any | Valor del elemento actual |
| `índice` | number | Posición (0-based) del elemento |
| `array` | Array | Referencia al array sobre el que se itera |

| Aspecto | Detalle |
|---|---|
| Valor de retorno del método | `undefined` (siempre) |
| Muta el array | No (salvo que el callback lo haga) |
| Argumento `thisArg` | Opcional; establece el `this` dentro del callback |
| Salta huecos (array sparse) | Sí, los salta |

## Ejemplos

### Registrar una lista de elementos DOM

```js
document.querySelectorAll(".card").forEach((card, i) => {
  card.setAttribute("data-index", i);
  card.addEventListener("click", () => console.log(`Card ${i} clicked`));
});
```

### Disparar efectos secundarios con acumulador externo

```js
const resumen = { total: 0, errores: [] };

pedidos.forEach((pedido) => {
  resumen.total += pedido.monto;
  if (pedido.estado === "error") resumen.errores.push(pedido.id);
});
```

### Con `thisArg` para mantener contexto

```js
class Logger {
  constructor(prefijo) { this.prefijo = prefijo; }
  log(msg) { console.log(`[${this.prefijo}] ${msg}`); }
}

const logger = new Logger("APP");
["inicio", "proceso", "fin"].forEach(function(evento) {
  this.log(evento);   // this = logger por el segundo argumento
}, logger);
```

## Recetas comunes

### Procesar respuestas de API en batch

```js
const errores = [];
respuestas.forEach((resp) => {
  if (!resp.ok) errores.push(resp.url);
  actualizarUI(resp);
});
if (errores.length) notificarErrores(errores);
```

### Construir un mapa de índices (alternativa a `Object.fromEntries`)

```js
const porId = {};
usuarios.forEach((u) => { porId[u.id] = u; });
```

### Comparación forEach vs for...of para elegir

```js
// forEach: cuando no necesitas break ni await
elementos.forEach((el) => procesar(el));

// for...of: cuando necesitas break o async/await
for (const el of elementos) {
  if (condicion(el)) break;
  await procesarAsync(el);
}
```

## Cómo funciona por dentro

El motor visita cada índice del array desde `0` hasta `length - 1`. Para cada índice que existe en el objeto (propiedad propia), invoca el callback con `(array[i], i, array)`. Los índices que no son propiedades propias del array (huecos en arrays sparse) se **saltan**. El valor de retorno del callback se descarta. Cuando termina la iteración, el método devuelve `undefined`.

El array no se "congela" durante la iteración: si el callback modifica la longitud o añade elementos más allá del índice actual, esos cambios pueden afectar qué índices se visitan, por lo que se desaconseja mutar el array dentro del forEach.

```js
// Demostración de salto de huecos
const sparse = [1, , , 4];  // índices 1 y 2 son huecos
sparse.forEach((v, i) => console.log(i, v));
// 0 1
// 3 4   ← los huecos (1, 2) se saltaron
```

> [!tip] Buenas prácticas
> - Usar `forEach` únicamente para efectos secundarios (logging, mutación de objetos externos, manipulación de DOM). Si necesitas un resultado, usa `map`, `filter` o `reduce`.
> - No intentar "romper" un `forEach` con `return` — el `return` solo sale del callback, no del `forEach`. Para poder usar `break`, usar `for...of`.
> - Para operaciones asíncronas con `await`, `forEach` no funciona correctamente: el callback es síncrono desde la perspectiva del método. Usar `for...of` con `await` o `Promise.all` con `map`.
> - Preferir nombres de parámetro descriptivos: `forEach((usuario, i) => ...)` es más claro que `forEach((x, i) => ...)`.

> [!warning] Errores comunes
> - **Intentar usar el valor de retorno:** `const result = arr.forEach(...)` siempre es `undefined`. Si necesitas valor de retorno, el método correcto es otro.
> - **`async/await` dentro de forEach:** `arr.forEach(async (x) => await fetch(x))` no espera las promesas — todas se disparan en paralelo y `forEach` retorna antes de que terminen. Usar `for...of` o `await Promise.all(arr.map(async ...))`.
> - **Olvidar que no hay `break`:** intentar lanzar una excepción para salir es un antipatrón; indica que la herramienta correcta es `find`, `some` o un bucle imperativo.
> - **Confundir con `map`:** si el callback devuelve un valor y esperas un array de resultados, el resultado se descarta; debes usar `map`.

## Notas relacionadas

- [[index|Métodos de Array Funcionales — Índice]]
- [[02 map]]
- [[03 filter]]
- [[12 Encadenamiento de Métodos]]
