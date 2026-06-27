---
title: DataView — Vista con control de tipo y endianness
aliases:
  - DataView
  - endianness javascript
  - parseo binario javascript
tags:
  - javascript
  - api/clase
  - estructuras-datos
objeto: DataView
tipo: clase
draft: false
order: 2
---

# DataView

> [!definicion]
> `DataView` es una vista sobre un `ArrayBuffer` que permite leer y escribir valores de **cualquier tipo numérico en cualquier posición del buffer**, con control explícito del **endianness** (orden de bytes) por operación. A diferencia de los TypedArray (que son homogéneos), DataView permite mezclar distintos tipos en el mismo buffer y es la herramienta correcta para parsear protocolos de red, formatos de archivo binario o datos con estructura heterogénea.

```js
const buf = new ArrayBuffer(8);
const dv  = new DataView(buf);

// Escribir un Int32 en big-endian en el byte 0
dv.setInt32(0, 0x01020304, false); // false = big-endian

// Leer de vuelta
console.log(dv.getInt32(0, false).toString(16)); // → "1020304"

// Leer los mismos bytes como dos Int16 big-endian
console.log(dv.getInt16(0, false).toString(16)); // → "102"
console.log(dv.getInt16(2, false).toString(16)); // → "304"
```

## Firma del constructor

```js
new DataView(buffer, byteOffset?, byteLength?)
// buffer     — el ArrayBuffer sobre el que actuar
// byteOffset — byte de inicio (default: 0)
// byteLength — número de bytes de la vista (default: hasta el final del buffer)
```

```js
const buf = new ArrayBuffer(16);

const dvTotal = new DataView(buf);          // toda la longitud
const dvParcial = new DataView(buf, 4, 8); // bytes 4 a 11

console.log(dvTotal.byteLength);    // → 16
console.log(dvParcial.byteLength);  // → 8
console.log(dvParcial.byteOffset);  // → 4
```

## Métodos de lectura

Todos reciben `(byteOffset, littleEndian?)`. `littleEndian` es `false` (big-endian) por defecto para los métodos multi-byte; `Int8`/`Uint8` no tienen segundo argumento porque son de 1 byte.

| Método | Bytes | Rango |
|--------|-------|-------|
| `getInt8(offset)` | 1 | −128 a 127 |
| `getUint8(offset)` | 1 | 0 a 255 |
| `getInt16(offset, le?)` | 2 | −32768 a 32767 |
| `getUint16(offset, le?)` | 2 | 0 a 65535 |
| `getInt32(offset, le?)` | 4 | −2147483648 a 2147483647 |
| `getUint32(offset, le?)` | 4 | 0 a 4294967295 |
| `getFloat32(offset, le?)` | 4 | ±3.4e38 |
| `getFloat64(offset, le?)` | 8 | ±1.8e308 |

```js
const buf = new ArrayBuffer(10);
const dv = new DataView(buf);

dv.setUint8(0, 0xAB);
dv.setInt16(1, -500, true);     // little-endian
dv.setFloat32(3, 3.14, true);   // little-endian
dv.setUint8(7, 0xFF);

console.log(dv.getUint8(0));          // → 171  (0xAB)
console.log(dv.getInt16(1, true));    // → -500
console.log(dv.getFloat32(3, true));  // → 3.14 (aprox, precisión simple)
console.log(dv.getUint8(7));          // → 255
```

## Métodos de escritura

Misma signatura que los métodos de lectura, con el valor como segundo argumento (o tercero para los multi-byte):

| Método | Firma |
|--------|-------|
| `setInt8` | `setInt8(offset, valor)` |
| `setUint8` | `setUint8(offset, valor)` |
| `setInt16` | `setInt16(offset, valor, littleEndian?)` |
| `setUint16` | `setUint16(offset, valor, littleEndian?)` |
| `setInt32` | `setInt32(offset, valor, littleEndian?)` |
| `setUint32` | `setUint32(offset, valor, littleEndian?)` |
| `setFloat32` | `setFloat32(offset, valor, littleEndian?)` |
| `setFloat64` | `setFloat64(offset, valor, littleEndian?)` |

## Endianness — orden de bytes

**Endianness** describe en qué orden se almacenan los bytes de un valor multi-byte.

- **Big-endian** (red, archivo): el byte **más** significativo primero. `0x12345678` → `12 34 56 78`.
- **Little-endian** (x86, ARM moderno): el byte **menos** significativo primero. `0x12345678` → `78 56 34 12`.

El hardware de la mayoría de PCs y móviles modernos es little-endian. Los protocolos de red (TCP/IP, HTTP/2, WebSocket frames) usan big-endian (llamado "network byte order").

```js
const buf = new ArrayBuffer(4);
const dv  = new DataView(buf);
const u8  = new Uint8Array(buf);

// Escribir 0x01020304 en big-endian
dv.setUint32(0, 0x01020304, false);
console.log([...u8].map(b => b.toString(16))); // → ["1", "2", "3", "4"]  (big-endian)

// Escribir el mismo número en little-endian
dv.setUint32(0, 0x01020304, true);
console.log([...u8].map(b => b.toString(16))); // → ["4", "3", "2", "1"]  (little-endian)
```

## Cuándo usar DataView vs TypedArray

| Situación | Usar |
|-----------|------|
| Buffer homogéneo de un solo tipo numérico | TypedArray |
| Buffer con tipos mixtos en posiciones específicas | DataView |
| Control de endianness por operación | DataView |
| WebGL (vértices, índices, colores — buffers homogéneos) | TypedArray |
| Parseo de cabeceras de protocolo de red | DataView |
| Lectura de formatos de archivo binario (PNG, WAV, etc.) | DataView |

## Recetas comunes

### Parsear una cabecera de protocolo

```js
// Protocolo ficticio: [version: uint8][flags: uint8][length: uint32 BE][checksum: uint16 BE]
function parsearCabecera(buffer) {
  const dv = new DataView(buffer);
  return {
    version:  dv.getUint8(0),
    flags:    dv.getUint8(1),
    length:   dv.getUint32(2, false), // big-endian (network byte order)
    checksum: dv.getUint16(6, false),
  };
}

const buf = new ArrayBuffer(8);
const dv = new DataView(buf);
dv.setUint8(0, 2);         // version 2
dv.setUint8(1, 0b00000011); // flags
dv.setUint32(2, 1024, false); // length 1024
dv.setUint16(6, 0xABCD, false); // checksum

console.log(parsearCabecera(buf));
// → { version: 2, flags: 3, length: 1024, checksum: 43981 }
```

### Construir un mensaje binario

```js
function crearMensaje(tipo, payload) {
  const buf = new ArrayBuffer(4 + payload.byteLength);
  const dv  = new DataView(buf);
  dv.setUint16(0, tipo, true);           // tipo en little-endian
  dv.setUint16(2, payload.byteLength, true); // longitud del payload
  new Uint8Array(buf, 4).set(new Uint8Array(payload)); // copia el payload
  return buf;
}
```

## Cómo funciona por dentro

`DataView` almacena internamente un puntero al `ArrayBuffer`, un `byteOffset` y un `byteLength`. Cada llamada a `getXxx` o `setXxx` calcula `buffer_ptr + byteOffset + offset_argumento`, lee/escribe el número de bytes correspondiente al tipo, y aplica la conversión de endianness si es necesario — todo en código nativo del motor. No hay boxing de valores JS para cada byte; el motor trabaja directamente con la memoria del buffer.

> [!tip] Buenas prácticas
> - Definir constantes para los offsets de cada campo de un protocolo — hace el código legible y resistente a errores.
> - Al parsear datos de red, siempre especificar explícitamente el endianness (`false` = big-endian para red) aunque sea el default — hace el código autodocumentado.
> - Reutilizar un `DataView` para el mismo buffer en vez de crear uno nuevo en cada operación.

> [!warning] Errores comunes
> - **Omitir el argumento `littleEndian`:** el default es `false` (big-endian). En datos de red es correcto; para datos del sistema local (little-endian) hay que pasar `true`.
> - **`byteOffset` fuera de rango:** si `byteOffset + tamañoTipo > byteLength`, lanza `RangeError`.
> - **Usar DataView cuando el buffer es homogéneo:** para un array de 1000 floats, `Float32Array` es más conciso y normalmente más rápido (el motor puede optimizarlo mejor que las llamadas individuales a DataView).

## Notas relacionadas

- [[05 Typed Arrays/index|Typed Arrays — índice]]
- [[01 ArrayBuffer]]
- [[03 Vistas TypedArray]]
