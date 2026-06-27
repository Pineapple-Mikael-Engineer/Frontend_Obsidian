---
title: ArrayBuffer — Bloque de memoria binaria
aliases:
  - ArrayBuffer
  - buffer binario javascript
tags:
  - javascript
  - api/clase
  - estructuras-datos
objeto: ArrayBuffer
tipo: clase
draft: false
order: 1
---

# ArrayBuffer

> [!definicion]
> `ArrayBuffer` es un bloque de memoria de longitud fija en bytes, inicializado a ceros. Es el almacenamiento crudo subyacente de los Typed Arrays — no se puede leer ni escribir directamente sobre él. Para acceder a sus datos hay que crear una **vista** (`TypedArray` o `DataView`). La longitud es inmutable una vez creado.

```js
// Alocar 16 bytes
const buffer = new ArrayBuffer(16);

console.log(buffer.byteLength); // → 16

// No se puede leer directamente — hay que crear una vista
const vista = new Uint8Array(buffer);
vista[0] = 255;
vista[1] = 128;

console.log(vista[0]); // → 255
console.log(vista[1]); // → 128
```

## Firma del constructor

```js
new ArrayBuffer(byteLength)
// byteLength — número de bytes a alocar (entero ≥ 0)
// Lanza RangeError si byteLength es negativo o mayor que el límite del motor
```

## Propiedades y métodos

```js
buffer.byteLength              // → number — longitud en bytes (inmutable)
buffer.slice(inicio, fin?)     // → nuevo ArrayBuffer con copia del rango
ArrayBuffer.isView(valor)      // → boolean — ¿es una vista sobre algún buffer?
```

## `byteLength` — longitud en bytes

`byteLength` es una propiedad de solo lectura. No cambia tras la creación — los ArrayBuffers son de tamaño fijo (a diferencia de los arrays JS normales).

```js
const buf = new ArrayBuffer(8);
console.log(buf.byteLength); // → 8

// Intentar modificar no hace nada en strict mode y lanza en some engines
// buf.byteLength = 16; → TypeError en strict
```

## `slice` — copiar un rango

`slice` crea un **nuevo** ArrayBuffer con una copia del rango de bytes `[inicio, fin)`. No muta el buffer original. Equivalente conceptual a `Array.prototype.slice`.

```js
const buf = new ArrayBuffer(8);
const vista = new Uint8Array(buf);
vista.set([10, 20, 30, 40, 50, 60, 70, 80]);

const copia = buf.slice(2, 6); // bytes 2, 3, 4, 5
const vistaCopia = new Uint8Array(copia);
console.log([...vistaCopia]); // → [30, 40, 50, 60]

// El original no cambia
console.log([...vista]); // → [10, 20, 30, 40, 50, 60, 70, 80]
```

## `ArrayBuffer.isView` — comprobar si es una vista

`isView` retorna `true` para cualquier `TypedArray` o `DataView`. Útil para guardianes de tipo en funciones que aceptan buffers o vistas.

```js
console.log(ArrayBuffer.isView(new Uint8Array(4)));    // → true
console.log(ArrayBuffer.isView(new DataView(new ArrayBuffer(4)))); // → true
console.log(ArrayBuffer.isView(new ArrayBuffer(4)));   // → false  (buffer, no vista)
console.log(ArrayBuffer.isView([1, 2, 3]));            // → false  (array normal)
console.log(ArrayBuffer.isView("texto"));              // → false
```

## Crear un buffer desde datos

```js
// Desde array de bytes
const datos = [72, 101, 108, 108, 111]; // "Hello" en ASCII
const buf = new Uint8Array(datos).buffer;
console.log(new TextDecoder().decode(buf)); // → "Hello"

// Desde string con TextEncoder
const encoder = new TextEncoder();
const encoded = encoder.encode("Hola mundo"); // Uint8Array
const buffer2 = encoded.buffer;              // el ArrayBuffer subyacente
```

## Vistas sobre el mismo buffer

Múltiples vistas pueden compartir el mismo buffer. Escribir en una vista es visible en las demás — comparten la misma memoria física.

```js
const buf = new ArrayBuffer(4);
const u8  = new Uint8Array(buf);
const u16 = new Uint16Array(buf);
const u32 = new Uint32Array(buf);

u8[0] = 0xFF; // escribe en el primer byte
u8[1] = 0x00;
u8[2] = 0x00;
u8[3] = 0x00;

// u16[0] lee los bytes 0-1 como un entero de 16 bits (little-endian en x86/ARM)
console.log(u16[0]); // → 255  (0x00FF en little-endian)
console.log(u32[0]); // → 255  (0x000000FF)
```

## `SharedArrayBuffer` — memoria compartida entre Workers

`SharedArrayBuffer` es una variante de `ArrayBuffer` que puede ser compartida entre el hilo principal y `Worker`s sin copiar los datos. Requiere que el servidor envíe las cabeceras de seguridad `Cross-Origin-Opener-Policy: same-origin` y `Cross-Origin-Embedder-Policy: require-corp`.

```js
// En el hilo principal
const sab = new SharedArrayBuffer(4);
const vista = new Int32Array(sab);
vista[0] = 42;

const worker = new Worker("worker.js");
worker.postMessage(sab); // NO se copia — se comparte la memoria

// En worker.js
self.onmessage = (e) => {
  const compartido = new Int32Array(e.data);
  console.log(Atomics.load(compartido, 0)); // → 42
  // Atomics.* para acceso sincronizado entre hilos
};
```

## Cómo funciona por dentro

El motor aloca el bloque en el **heap nativo** (fuera del heap JS gestionado con su boxing/unboxing de valores). Es un bloque de bytes contiguos en memoria, accesibles directamente mediante puntero. Cuando se crea una vista `TypedArray`, el motor almacena internamente un puntero al inicio del buffer, un `byteOffset` y una longitud — el acceso a `ta[i]` es una operación aritmética de puntero directa: `buffer_ptr + byteOffset + i * bytesPerElement`. Esto elimina la indirección y el boxing de los arrays JS normales y permite velocidades cercanas a C/Wasm.

> [!tip] Buenas prácticas
> - Crear el `ArrayBuffer` con el tamaño exacto necesario desde el principio — es inmutable y no se puede redimensionar (salvo `ResizableArrayBuffer` en ES2024).
> - Reutilizar buffers en bucles de procesamiento de alto rendimiento en lugar de alocar nuevos en cada iteración.
> - Usar `ArrayBuffer.isView` en funciones públicas para validar que el argumento es una vista y no un buffer raw (que no es directamente legible).

> [!warning] Errores comunes
> - **Intentar leer directamente del buffer:** `buffer[0]` devuelve `undefined` — se necesita una vista.
> - **Confundir `byteLength` de buffer con `length` de TypedArray:** `buffer.byteLength` es bytes; `ta.length` es número de elementos del tipo de la vista.
> - **Usar `SharedArrayBuffer` sin las cabeceras COOP/COEP:** lanza `ReferenceError: SharedArrayBuffer is not defined` en browsers modernos por razones de seguridad (Spectre).

## Notas relacionadas

- [[05 Typed Arrays/index|Typed Arrays — índice]]
- [[02 DataView]]
- [[03 Vistas TypedArray]]
