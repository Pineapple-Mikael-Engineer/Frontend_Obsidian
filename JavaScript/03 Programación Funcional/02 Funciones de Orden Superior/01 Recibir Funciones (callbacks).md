---
title: Recibir Funciones (callbacks)
aliases:
  - callbacks
  - callback pattern
  - HOF callback
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
---

# Recibir Funciones (callbacks)

> [!definicion]
> Un **callback** es una función pasada como argumento a otra función para que esta la invoque en el momento apropiado. Cuando una HOF recibe un callback, delega en él la parte variable del comportamiento: la HOF define la estructura (cuándo y cómo llamar), y el callback define la lógica (qué hacer).

```js
function aplicarDosVeces(fn, x) {
  return fn(fn(x));
}

aplicarDosVeces(x => x * 2, 3);   // 12  → fn(fn(3)) = fn(6) = 12
aplicarDosVeces(x => x + 10, 5);  // 25  → fn(fn(5)) = fn(15) = 25
```

## Callbacks síncronos vs asíncronos

| Tipo | Cuándo se ejecuta | Casos de uso |
|---|---|---|
| Síncrono | Inmediatamente, dentro del call stack actual | `forEach`, `map`, `filter`, `sort`, `Array.from` |
| Asíncrono | Más tarde, tras un evento o tarea I/O | `setTimeout`, `addEventListener`, `fs.readFile`, XHR |

### Síncrono

El callback se llama dentro de la misma invocación de la HOF. El flujo del programa no se interrumpe:

```js
// forEach llama al callback por cada elemento, de forma síncrona
[10, 20, 30].forEach(n => console.log(n));
// 10
// 20
// 30
console.log("Esto se ejecuta después de forEach, como se espera");
```

### Asíncrono

El callback se encola y se ejecuta más tarde, cuando el motor lo programa. El código que sigue a la llamada continúa sin esperar:

```js
setTimeout(() => console.log("Tardío"), 1000);
console.log("Inmediato");
// Imprime:
// "Inmediato"
// "Tardío"  (1 segundo después)
```

## HOFs de Array: signaturas de callback

Los métodos nativos de `Array` tienen signaturas fijas que la HOF controla internamente. Conocer la firma evita bugs sutiles.

| Método | Firma del callback | Lo que devuelve la HOF |
|---|---|---|
| `map` | `(elem, idx, arr) => nuevoElem` | Nuevo array de igual longitud |
| `filter` | `(elem, idx, arr) => boolean` | Nuevo array con los elementos que devuelven `true` |
| `reduce` | `(acum, elem, idx, arr) => nuevoAcum` | Valor acumulado final |
| `forEach` | `(elem, idx, arr) => void` | `undefined` siempre |
| `find` | `(elem, idx, arr) => boolean` | Primer elemento que cumple, o `undefined` |
| `some` | `(elem, idx, arr) => boolean` | `true` si al menos uno cumple |
| `every` | `(elem, idx, arr) => boolean` | `true` si todos cumplen |
| `sort` | `(a, b) => número` | El array original reordenado in-place |
| `flatMap` | `(elem, idx, arr) => elem o array` | Nuevo array mapeado y aplanado un nivel |

```js
const personas = [
  { nombre: "Ana",   edad: 30 },
  { nombre: "Luis",  edad: 17 },
  { nombre: "María", edad: 25 },
];

// map: extraer nombres
personas.map(p => p.nombre);               // ["Ana", "Luis", "María"]

// filter: solo mayores de edad
personas.filter(p => p.edad >= 18);        // [Ana, María]

// reduce: sumar edades
personas.reduce((sum, p) => sum + p.edad, 0); // 72

// sort: por edad ascendente (comparador numérico)
[...personas].sort((a, b) => a.edad - b.edad);
// [Luis 17, María 25, Ana 30]
```

## La signatura de `sort` importa

`Array.sort` es un caso especial: espera que el comparador devuelva un **número** (negativo, cero o positivo), no un booleano. Usar un callback booleano produce resultados incorrectos o dependientes del motor:

```js
const nums = [10, 2, 30, 4];

// INCORRECTO: el callback devuelve boolean, no número
nums.sort((a, b) => a > b);   // orden indefinido / incorrecto

// CORRECTO: diferencia numérica
nums.sort((a, b) => a - b);   // [2, 4, 10, 30]
```

## Implementar una HOF que recibe callback

Crear una HOF propia es tan simple como recibir la función en un parámetro e invocarla:

```js
// HOF de transformación genérica
function transformar(array, fn) {
  const resultado = [];
  for (const elem of array) {
    resultado.push(fn(elem));
  }
  return resultado;
}

transformar([1, 2, 3], x => x ** 2);   // [1, 4, 9]
transformar(["a","b"], s => s.toUpperCase()); // ["A", "B"]
```

### HOF con múltiples callbacks

```js
// Similar a Array.reduce pero con acceso a índice
function reducirConÍndice(array, fn, inicial) {
  let acum = inicial;
  for (let i = 0; i < array.length; i++) {
    acum = fn(acum, array[i], i);
  }
  return acum;
}

reducirConÍndice([10, 20, 30], (ac, x, i) => `${ac}[${i}:${x}]`, "");
// "[0:10][1:20][2:30]"
```

## Abstracción de comportamiento: crearValidador

Una HOF que acepta un array de funciones predicado y devuelve un validador que las aplica todas:

```js
function crearValidador(reglas) {
  return function validar(valor) {
    return reglas.every(regla => regla(valor));
  };
}

const validarEmail = crearValidador([
  v => typeof v === "string",
  v => v.includes("@"),
  v => v.length > 5,
]);

validarEmail("usuario@ejemplo.com");  // true
validarEmail("no-es-email");          // false
```

## Recetas comunes

### pipeline síncrona

Pasa un valor por una serie de funciones transformadoras en secuencia:

```js
function pipeline(valor, ...fns) {
  return fns.reduce((v, fn) => fn(v), valor);
}

const resultado = pipeline(
  "  Hola Mundo  ",
  s => s.trim(),
  s => s.toLowerCase(),
  s => s.replace(/\s+/g, "-"),
);
// "hola-mundo"
```

### once(fn)

Devuelve una función que invoca `fn` solo la primera vez; en llamadas posteriores devuelve el resultado anterior:

```js
function once(fn) {
  let llamado   = false;
  let resultado;
  return function(...args) {
    if (!llamado) {
      llamado   = true;
      resultado = fn.apply(this, args);
    }
    return resultado;
  };
}

const inicializar = once(() => {
  console.log("Inicializando...");
  return 42;
});

inicializar(); // "Inicializando..." → 42
inicializar(); // silencioso          → 42
inicializar(); // silencioso          → 42
```

### Callback hell y la salida

El anidamiento profundo de callbacks asíncronos — conocido como _callback hell_ — vuelve el código difícil de leer y manejar:

```js
// Callback hell
fs.readFile("a.txt", (err, a) => {
  fs.readFile("b.txt", (err, b) => {
    fs.readFile("c.txt", (err, c) => {
      procesarTodo(a, b, c);
    });
  });
});

// Con Promises: plano y legible
const a = await fs.promises.readFile("a.txt");
const b = await fs.promises.readFile("b.txt");
const c = await fs.promises.readFile("c.txt");
procesarTodo(a, b, c);
```

Las Promises y `async/await` son la respuesta estándar al callback hell, pero el patrón callback sigue siendo fundamental para entender cómo funcionan internamente.

## Cómo funciona por dentro

Pasar una función como argumento transfiere una **referencia** al objeto `Function` en el heap — no copia la función. La HOF receptora la almacena en su variable de parámetro local y la invoca con `fn(args)` o `fn.apply(ctx, args)` cuando corresponde.

```js
function ejecutar(fn) {
  //  fn es una referencia al mismo objeto Function del caller
  //  typeof fn === "function"  → true
  //  fn.name, fn.length, etc. son accesibles
  console.log(`Ejecutando: ${fn.name}`);
  return fn();
}

function saludar() { return "Hola"; }
ejecutar(saludar); // "Ejecutando: saludar" → "Hola"
```

Cuando la HOF termina, la variable de parámetro desaparece, pero el objeto `Function` en el heap sigue vivo mientras haya alguna referencia externa a él. Si la HOF almacena el callback en una estructura persistente (lista de listeners, timer activo), la función no será recogida por el GC — lo que puede causar memory leaks si no se limpia.

> [!tip] Buenas prácticas
> - Nombra tus callbacks incluso cuando los pases como funciones flecha: `arr.map(function duplicar(x) { return x * 2; })` — el nombre aparece en stack traces.
> - Valida que el argumento es una función antes de invocar: `if (typeof fn !== "function") throw new TypeError(...)`.
> - Para callbacks asíncronos, maneja siempre el error: en el patrón Node.js, el primer parámetro del callback es siempre `error`.
> - Prefiere `Promise` / `async-await` sobre callbacks anidados para operaciones asíncronas en serie.

> [!warning] Errores comunes
> - **Invocar en lugar de pasar**: `arr.map(fn())` pasa el _resultado_ de `fn`, no la función. Lo correcto: `arr.map(fn)`.
> - **Ignorar el segundo y tercer argumento** del callback de `map`/`filter`: `["1","2","3"].map(parseInt)` devuelve `[1, NaN, NaN]` porque `parseInt` recibe `(elem, idx)` y usa el índice como radix.
> - **Asumir orden en operaciones async paralelas**: `Promise.all` garantiza el orden del resultado; callbacks independientes en `setTimeout` no.
> - **this incorrecto en callbacks de método**: `arr.forEach(this.procesar)` pierde el contexto de `this`; usa `.bind(this)` o arrow function.

## Notas relacionadas

- [[index]]
- [[02 Retornar Funciones]]
