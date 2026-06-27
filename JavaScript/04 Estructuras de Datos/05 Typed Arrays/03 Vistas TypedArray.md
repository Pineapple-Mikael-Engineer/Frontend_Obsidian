---
title: Vistas TypedArray — Int8Array, Uint8Array, Float32Array…
aliases:
  - Int8Array
  - Uint8Array
  - Uint8ClampedArray
  - Int16Array
  - Uint16Array
  - Int32Array
  - Uint32Array
  - Float32Array
  - Float64Array
  - TypedArray tipos
tags:
  - javascript
  - api/clase
  - estructuras-datos
objeto: TypedArray
tipo: clase
draft: false
order: 3
---

# Vistas TypedArray

> [!definicion]
> Las vistas TypedArray son clases concretas que presentan un `ArrayBuffer` como un array de elementos de tipo fijo. Cada elemento tiene el mismo tamaño en bytes y el acceso por índice (`ta[i]`) es una operación directa sobre la memoria — sin boxing ni indirección. Hay 9 tipos. La asignación de valores fuera del rango no lanza error: trunca o envuelve según el tipo (excepción: `Uint8ClampedArray`, que fija al rango 0–255).

```js
const ta = new Int32Array(4); // 4 elementos × 4 bytes = 16 bytes
ta[0] = 100;
ta[1] = -200;
ta[2] = 2147483647; // INT_MAX

console.log(ta[0]);          // → 100
console.log(ta[1]);          // → -200
console.log(ta[2]);          // → 2147483647
console.log(ta.length);      // → 4
console.log(ta.byteLength);  // → 16
```

## Los 9 tipos de TypedArray

| Tipo | Bytes/elem | Rango | Desbordamiento |
|------|-----------|-------|----------------|
| `Int8Array` | 1 | −128 a 127 | Envuelve (wrap) |
| `Uint8Array` | 1 | 0 a 255 | Envuelve (wrap) |
| `Uint8ClampedArray` | 1 | 0 a 255 | Fija al límite (clamp) |
| `Int16Array` | 2 | −32768 a 32767 | Envuelve (wrap) |
| `Uint16Array` | 2 | 0 a 65535 | Envuelve (wrap) |
| `Int32Array` | 4 | −2147483648 a 2147483647 | Envuelve (wrap) |
| `Uint32Array` | 4 | 0 a 4294967295 | Envuelve (wrap) |
| `Float32Array` | 4 | ±3.4e38 (IEEE 754 single) | NaN/Infinity |
| `Float64Array` | 8 | ±1.8e308 (IEEE 754 double) | NaN/Infinity |

## Formas de crear un TypedArray

```js
// 1. Con longitud — elementos inicializados a 0
const ta1 = new Float32Array(8);    // 8 floats, 32 bytes

// 2. Desde un ArrayBuffer existente
const buf = new ArrayBuffer(16);
const ta2 = new Float32Array(buf);  // 4 floats (16 bytes / 4)

// 3. Desde buffer con offset y longitud
const ta3 = new Float32Array(buf, 4, 2); // 2 floats desde byte 4

// 4. Desde array normal — convierte y copia
const ta4 = new Uint8Array([10, 20, 30, 40]);

// 5. Desde otro TypedArray — convierte tipos
const int32 = new Int32Array([100, 200, 300]);
const float64 = new Float64Array(int32); // conversión de tipos
console.log([...float64]); // → [100, 200, 300]  (como float64)
```

## Propiedades de instancia

```js
const ta = new Int16Array(new ArrayBuffer(10), 2, 4); // offset=2, length=4

ta.length      // → 4   (número de elementos)
ta.byteLength  // → 8   (bytes que ocupa la vista: 4 × 2)
ta.byteOffset  // → 2   (byte de inicio en el buffer)
ta.buffer      // → el ArrayBuffer subyacente
```

## Acceso y desbordamiento

El desbordamiento no lanza error — los bits que no caben se descartan (wrap en enteros) o se clampean (`Uint8ClampedArray`).

```js
const u8 = new Uint8Array(1);
u8[0] = 255;
console.log(u8[0]); // → 255

u8[0] = 256;        // 256 = 0x100 → los 8 bits bajos son 0x00
console.log(u8[0]); // → 0  (envuelve)

u8[0] = 300;        // 300 = 0x12C → 8 bits bajos = 0x2C = 44
console.log(u8[0]); // → 44

u8[0] = -1;         // -1 en complemento a 2 = 0xFF = 255
console.log(u8[0]); // → 255

// Uint8ClampedArray — clampea en lugar de envolver
const uc = new Uint8ClampedArray(1);
uc[0] = 300;        // → 255  (fijado al máximo)
uc[0] = -5;         // → 0    (fijado al mínimo)
console.log(uc[0]); // → 0
```

## `Uint8ClampedArray` — píxeles de Canvas

`Uint8ClampedArray` existe específicamente para los datos de píxeles de `ImageData` en `<canvas>`. Cada componente RGBA es un entero 0–255 y la clampeada evita corrupción de datos al hacer aritmética de color.

```js
const canvas = document.getElementById("miCanvas");
const ctx = canvas.getContext("2d");
const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);

// imageData.data es un Uint8ClampedArray
const data = imageData.data; // [R, G, B, A, R, G, B, A, ...]

// Invertir colores
for (let i = 0; i < data.length; i += 4) {
  data[i]     = 255 - data[i];     // R
  data[i + 1] = 255 - data[i + 1]; // G
  data[i + 2] = 255 - data[i + 2]; // B
  // data[i + 3] es el canal alpha — se deja igual
}

ctx.putImageData(imageData, 0, 0);
```

## Métodos disponibles

Los TypedArray implementan un subconjunto de los métodos de `Array.prototype`. Los métodos que devuelven un array nuevo devuelven un TypedArray del mismo tipo.

```js
const ta = new Float32Array([1.5, 2.5, 3.5, 4.5]);

// Métodos de Array disponibles
ta.map(x => x * 2)          // → Float32Array [3, 5, 7, 9]
ta.filter(x => x > 2)       // → Float32Array [2.5, 3.5, 4.5]
ta.reduce((acc, x) => acc + x, 0) // → 12
ta.slice(1, 3)              // → Float32Array [2.5, 3.5]  (copia)
ta.find(x => x > 3)         // → 3.5
ta.findIndex(x => x > 3)    // → 2
ta.includes(2.5)             // → true
ta.indexOf(2.5)              // → 1
ta.some(x => x > 4)         // → true
ta.every(x => x > 0)        // → true
ta.fill(0, 2, 4)            // muta: rellena índices 2 y 3 con 0
ta.reverse()                // muta: invierte in-place
ta.sort()                   // muta: ordena
ta.forEach(x => console.log(x)) // itera sin devolver
ta[Symbol.iterator]()       // iterable

// Métodos NO disponibles en TypedArray (solo en Array)
// push, pop, shift, unshift, splice, flat, flatMap, concat
```

## `set` — copiar datos de otro array

`ta.set(fuente, offset?)` copia datos de un array o TypedArray en el TypedArray destino. Es más eficiente que un bucle porque el motor puede usar `memcpy` a nivel nativo.

```js
const destino = new Uint8Array(8);
const fuente  = new Uint8Array([10, 20, 30, 40]);

destino.set(fuente);        // copia desde el byte 0
destino.set(fuente, 4);     // copia desde el byte 4

console.log([...destino]); // → [10, 20, 30, 40, 10, 20, 30, 40]

// Desde array normal
destino.set([1, 2, 3], 2);
console.log([...destino]); // → [10, 20, 1, 2, 3, 40, 10, 20]
```

## `subarray` — vista parcial sin copia

`ta.subarray(inicio, fin?)` crea un nuevo TypedArray del mismo tipo que apunta al mismo buffer (sin copiar). Cambios en el subarray se reflejan en el original.

```js
const ta = new Int32Array([1, 2, 3, 4, 5]);
const sub = ta.subarray(1, 4); // índices 1, 2, 3

console.log([...sub]); // → [2, 3, 4]
sub[0] = 99;
console.log(ta[1]);    // → 99  (mismo buffer — cambio visible)

// Contraste: slice sí copia
const copia = ta.slice(1, 4);
copia[0] = 0;
console.log(ta[1]);    // → 99  (no afecta al original)
```

## Recetas comunes

### Vértices 3D para WebGL

```js
// 3 vértices de un triángulo: x, y, z por vértice (9 floats)
const vertices = new Float32Array([
  0.0,  0.5, 0.0,  // vértice superior
 -0.5, -0.5, 0.0,  // inferior izquierdo
  0.5, -0.5, 0.0,  // inferior derecho
]);

// Subir al GPU
gl.bindBuffer(gl.ARRAY_BUFFER, buffer);
gl.bufferData(gl.ARRAY_BUFFER, vertices, gl.STATIC_DRAW);
```

### Procesar audio en Web Audio API

```js
const audioCtx = new AudioContext();
const sampleRate = audioCtx.sampleRate;
const duracion = 1; // segundo
const numSamples = sampleRate * duracion;

// AudioBuffer usa Float32Array internamente
const audioBuffer = audioCtx.createBuffer(1, numSamples, sampleRate);
const canal = audioBuffer.getChannelData(0); // Float32Array

// Generar onda sinusoidal a 440 Hz
for (let i = 0; i < numSamples; i++) {
  canal[i] = Math.sin(2 * Math.PI * 440 * i / sampleRate) * 0.5;
}
```

### Leer un archivo binario con FileReader

```js
const input = document.querySelector('input[type="file"]');
input.addEventListener("change", async (e) => {
  const file = e.target.files[0];
  const buffer = await file.arrayBuffer();
  const u8 = new Uint8Array(buffer);

  // Comprobar firma PNG (primeros 8 bytes)
  const firmaEsperada = [137, 80, 78, 71, 13, 10, 26, 10];
  const esPNG = firmaEsperada.every((byte, i) => u8[i] === byte);
  console.log("Es PNG:", esPNG);
});
```

## Cómo funciona por dentro

Una instancia TypedArray almacena tres valores internos: un puntero al `ArrayBuffer` subyacente, un `byteOffset` y una `length` (en elementos). El acceso `ta[i]` se traduce directamente a `*(buffer_ptr + byteOffset + i * BYTES_PER_ELEMENT)` — una operación de puntero en la representación nativa de la JIT. El motor evita el boxing JS (que envolvería cada número en un objeto heap) porque sabe que todos los elementos son del mismo tipo primitivo fijo. El `BYTES_PER_ELEMENT` es una constante estática del tipo de la clase — `Float32Array.BYTES_PER_ELEMENT === 4`.

```js
console.log(Int8Array.BYTES_PER_ELEMENT);   // → 1
console.log(Float32Array.BYTES_PER_ELEMENT); // → 4
console.log(Float64Array.BYTES_PER_ELEMENT); // → 8
```

> [!tip] Buenas prácticas
> - Usar `Float32Array` para geometría WebGL (precisión suficiente, mitad de memoria que `Float64Array`).
> - Usar `Uint8ClampedArray` siempre para datos de píxeles — es lo que devuelve `ImageData.data` y el motor lo optimiza.
> - Preferir `subarray` sobre `slice` cuando no necesitas una copia independiente — evita alocar un nuevo buffer.
> - Usar `set` para copias masivas de datos entre TypedArrays — es una `memcpy` nativa.

> [!warning] Errores comunes
> - **Esperar un error al asignar valores fuera del rango:** `u8[0] = 256` silenciosamente pone `0`. Esto puede causar corrupción de datos difícil de rastrear.
> - **Confundir `length` y `byteLength`:** `length` es elementos, `byteLength` es bytes. Para un `Int32Array(4)`, `length === 4` pero `byteLength === 16`.
> - **Usar métodos de Array no disponibles:** `ta.push(5)` lanza `TypeError`. Los TypedArray tienen tamaño fijo — no pueden crecer.
> - **Olvidar que `subarray` comparte el buffer:** modificar el resultado afecta al original. Si necesitas independencia, usar `slice`.

## Notas relacionadas

- [[05 Typed Arrays/index|Typed Arrays — índice]]
- [[01 ArrayBuffer]]
- [[02 DataView]]
