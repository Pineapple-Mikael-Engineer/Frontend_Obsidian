---
title: Typed Arrays — Vistas sobre memoria binaria
aliases:
  - Typed Arrays
  - TypedArray
  - arrays tipados javascript
tags:
  - javascript
  - api/clase
  - estructuras-datos
objeto: TypedArray
tipo: clase
draft: false
order: 5
---

# Typed Arrays

> [!definicion]
> Los Typed Arrays son **vistas sobre un `ArrayBuffer`** — un bloque de memoria binaria cruda de longitud fija. A diferencia de los arrays JS normales (que son objetos dinámicos con cualquier tipo de valor), un Typed Array tiene un tipo fijo para todos sus elementos (entero de 8 bits, flotante de 32 bits…) y accede directamente a la memoria subyacente sin boxing. Son esenciales para WebGL, Web Audio, WebSockets, `FileReader`, `fetch` con streams, `Canvas` (`ImageData`) y `SharedArrayBuffer`.

```js
// ArrayBuffer: bloque de 16 bytes
const buffer = new ArrayBuffer(16);

// Vista Int32Array: 4 enteros de 32 bits sobre ese buffer
const int32 = new Int32Array(buffer);
int32[0] = 100;
int32[1] = 200;
int32[2] = 300;
int32[3] = 400;

console.log(int32[0]);       // → 100
console.log(int32.length);   // → 4
console.log(int32.byteLength); // → 16
```

## Los 9 tipos de vista TypedArray

| Tipo | Bytes por elemento | Rango de valores | Uso típico |
|------|--------------------|------------------|------------|
| `Int8Array` | 1 | −128 a 127 | Datos de audio crudo signed |
| `Uint8Array` | 1 | 0 a 255 | Datos binarios generales |
| `Uint8ClampedArray` | 1 | 0 a 255 (clamp) | Píxeles de Canvas (ImageData) |
| `Int16Array` | 2 | −32768 a 32767 | Audio PCM 16-bit |
| `Uint16Array` | 2 | 0 a 65535 | Índices de geometría WebGL |
| `Int32Array` | 4 | −2147483648 a 2147483647 | Datos enteros de precisión |
| `Uint32Array` | 4 | 0 a 4294967295 | Índices grandes WebGL |
| `Float32Array` | 4 | ±3.4e38 (precisión simple) | Vértices y coordenadas WebGL |
| `Float64Array` | 8 | ±1.8e308 (precisión doble) | Cálculos científicos |

## Relación entre las partes

```
ArrayBuffer  ←  bloque de bytes en memoria
     ↑
     │ vista sobre
     │
TypedArray / DataView  ←  formas de leer/escribir el buffer
```

Un mismo buffer puede tener múltiples vistas simultáneas de tipos distintos. Cambios en una vista se reflejan en las demás porque comparten la misma memoria subyacente.

```js
const buf = new ArrayBuffer(4);
const u8  = new Uint8Array(buf);
const u32 = new Uint32Array(buf);

u8[0] = 0x01; u8[1] = 0x00; u8[2] = 0x00; u8[3] = 0x00;
console.log(u32[0]); // → 1  (en little-endian: byte 0 es el menos significativo)
```

## Notas relacionadas

- [[01 ArrayBuffer]]
- [[02 DataView]]
- [[03 Vistas TypedArray]]
- [[04 WeakMap y WeakSet/index|WeakMap y WeakSet — índice]]
