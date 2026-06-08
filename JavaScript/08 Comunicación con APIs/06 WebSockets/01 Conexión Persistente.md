---
title: WebSocket — Conexión Persistente
aliases:
  - new WebSocket
  - WebSocket.send
  - WebSocket.close
  - readyState WebSocket
tags:
  - javascript
  - api/clase
  - red
objeto: WebSocket
tipo: clase
retorna: WebSocket
muta: false
asincrono: false
draft: false
---

# Conexión Persistente — `WebSocket`

> [!definicion]
> `new WebSocket(url, protocolos?)` crea y abre una conexión WebSocket al servidor indicado. `url` usa el esquema `ws://` (texto plano) o `wss://` (cifrado TLS, equivalente a HTTPS). `protocolos` es un string o array de strings con los subprotocolos que el cliente ofrece negociar (ej. `"graphql-ws"`, `"mqtt"`); el servidor elige uno y queda registrado en `ws.protocol`. La conexión se inicia de forma asíncrona: el objeto se retorna de inmediato con `readyState === CONNECTING`.

```js
// Conexión segura con subprotocolo graphql-ws
const ws = new WebSocket('wss://api.ejemplo.com/graphql', 'graphql-ws');

// Verificar el subprotocolo negociado tras la apertura
ws.addEventListener('open', () => {
  console.log(ws.protocol); // → "graphql-ws" si el servidor lo aceptó
});
```

## `readyState` — estado de la conexión

| Valor | Constante | Descripción |
|---|---|---|
| `0` | `WebSocket.CONNECTING` | Conexión iniciada, handshake en curso |
| `1` | `WebSocket.OPEN` | Conexión abierta; `send()` disponible |
| `2` | `WebSocket.CLOSING` | Cierre iniciado, esperando confirmación |
| `3` | `WebSocket.CLOSED` | Conexión cerrada o no pudo abrirse |

Solo se puede llamar a `ws.send()` cuando `ws.readyState === WebSocket.OPEN` (valor `1`). Intentar enviar en cualquier otro estado lanza `InvalidStateError`.

## `ws.send(data)` — enviar datos

Encola un mensaje para enviar al servidor. El tipo de `data` determina el tipo de frame:

| Tipo de `data` | Frame WebSocket |
|---|---|
| `string` | Frame de texto (UTF-8) |
| `ArrayBuffer` | Frame binario |
| `TypedArray` / `DataView` | Frame binario |
| `Blob` | Frame binario |

```js
// Enviar JSON (string)
ws.send(JSON.stringify({ tipo: 'mensaje', contenido: 'Hola' }));

// Enviar datos binarios (ArrayBuffer)
const buffer = new Uint8Array([0x01, 0x02, 0x03]).buffer;
ws.send(buffer);
```

## `ws.close(code?, reason?)` — cerrar la conexión

Inicia el cierre limpio de la conexión (handshake de cierre de 4 pasos). `code` es un entero del estándar RFC 6455; `reason` es un string descriptivo (máx. 123 bytes UTF-8).

| Código | Significado |
|---|---|
| `1000` | Cierre normal (operación completada) |
| `1001` | El endpoint se va (navegador cierra pestaña, servidor reinicia) |
| `1008` | Violación de política (mensaje inaceptable) |
| `1011` | Error interno inesperado del servidor |

```js
// Cierre limpio desde el cliente
ws.close(1000, 'sesión terminada');
```

## Propiedades de diagnóstico

`ws.bufferedAmount` — número de bytes encolados con `send()` que aún no se han enviado a la red. Útil para control de flujo: si `bufferedAmount` crece sin límite, el cliente está enviando más rápido de lo que la red puede drenar.

```js
// Control de flujo: no encolar si hay datos pendientes
function enviarSiLibre(ws, data) {
  if (ws.bufferedAmount === 0) {
    ws.send(data);
  }
}
```

`ws.binaryType` — controla cómo se entregan los datos binarios al evento `message`. Valor `"blob"` (defecto) o `"arraybuffer"`. Cambiar a `"arraybuffer"` antes de recibir datos binarios si se necesita acceso sincrónico a los bytes.

```js
ws.binaryType = 'arraybuffer';
ws.addEventListener('message', (e) => {
  const view = new Uint8Array(e.data); // e.data es ArrayBuffer
  console.log(view[0]);
});
```

`ws.url` — la URL de conexión tal como se pasó al constructor (solo lectura).

`ws.protocol` — el subprotocolo negociado con el servidor, o string vacío si no se negoció ninguno (solo lectura).

## Cliente de chat básico

```js
function crearCliente(url, nombre) {
  const ws = new WebSocket(url);

  ws.addEventListener('open', () => {
    console.log('Conectado');
    ws.send(JSON.stringify({ tipo: 'unirse', nombre }));
  });

  ws.addEventListener('message', (e) => {
    const msg = JSON.parse(e.data);
    console.log(`[${msg.de}]: ${msg.texto}`);
  });

  ws.addEventListener('close', (e) => {
    console.log(`Desconectado (${e.code}): ${e.reason}`);
  });

  return {
    enviar(texto) {
      if (ws.readyState === WebSocket.OPEN) {
        ws.send(JSON.stringify({ tipo: 'mensaje', texto }));
      }
    },
    cerrar() {
      ws.close(1000, 'usuario salió');
    },
  };
}

const cliente = crearCliente('wss://chat.ejemplo.com', 'Mikael');
cliente.enviar('Hola a todos');
```

## Cómo funciona por dentro

El handshake inicial es HTTP/1.1: el cliente envía una petición `GET` con las cabeceras `Upgrade: websocket`, `Connection: Upgrade` y `Sec-WebSocket-Key` (nonce aleatorio en base64). El servidor responde `101 Switching Protocols` con `Sec-WebSocket-Accept` (el nonce hasheado con SHA-1). Tras el 101, el socket TCP se reutiliza pero el protocolo cambia a WebSocket: los datos viajan como frames binarios, no como peticiones HTTP. Cada frame tiene un opcode (texto, binario, ping, pong, close), un bit de mask (el cliente siempre enmascara sus frames), y la longitud del payload.

El servidor envía frames `ping` periódicamente para mantener viva la conexión y detectar clientes desconectados; el navegador responde con `pong` automáticamente. Este mecanismo de keepalive opera por debajo del API JavaScript — la aplicación no necesita gestionarlo.

> [!tip]
> Usar `wss://` siempre en producción. Además del cifrado, algunos proxies intermedios cierran conexiones `ws://` de larga duración porque las confunden con conexiones HTTP colgadas.

> [!warning]
> Llamar a `ws.send()` durante `readyState === CONNECTING` lanza `InvalidStateError` de forma síncrona. El lugar correcto para el primer `send()` es dentro del handler del evento `open`. Si la lógica de negocio necesita enviar antes de que la conexión abra, se puede acumular una cola y vaciarla en `open`.

## Notas relacionadas

- [[02 Eventos (open, message, error, close)|Eventos]] — ciclo de vida completo, `CloseEvent`, reconexión automática
- [[index|WebSockets — índice de sección]]
