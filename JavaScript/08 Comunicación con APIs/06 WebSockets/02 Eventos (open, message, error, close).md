---
title: WebSocket — Eventos (open, message, error, close)
aliases:
  - WebSocket eventos
  - ws.onmessage
  - ws.onopen
  - ws.onerror
  - ws.onclose
  - CloseEvent
  - MessageEvent WebSocket
tags:
  - javascript
  - api/clase
  - red
objeto: WebSocket
tipo: concepto
retorna: undefined
muta: false
asincrono: true
draft: false
order: 2
---

# Eventos WebSocket — `open`, `message`, `error`, `close`

> [!definicion]
> La conexión `WebSocket` expone su ciclo de vida a través de cuatro eventos del DOM. Se registran con `addEventListener` o con las propiedades `ws.onopen`, `ws.onmessage`, `ws.onerror` y `ws.onclose`. Los eventos se despachan en el orden en que ocurren: `open` → (`message` | `error`)* → `close`. El evento `error` siempre va seguido de `close`; no puede haber un `error` sin que la conexión se cierre acto seguido.

```js
const ws = new WebSocket('wss://api.ejemplo.com/eventos');

ws.addEventListener('open',    (e) => console.log('Conectado'));
ws.addEventListener('message', (e) => console.log('Dato:', e.data));
ws.addEventListener('error',   (e) => console.warn('Error — la conexión cerrará'));
ws.addEventListener('close',   (e) => console.log(`${e.code} ${e.reason} limpio=${e.wasClean}`));
```

## Tabla de eventos

| Evento | Tipo del objeto evento | Cuándo se dispara | Propiedades relevantes |
|---|---|---|---|
| `open` | `Event` | Handshake completado; `readyState` pasa a `OPEN` | — |
| `message` | `MessageEvent` | Llega un frame completo del servidor | `e.data`, `e.origin`, `e.lastEventId` |
| `error` | `Event` | Error de red o protocolo | Sin detalles (por seguridad) |
| `close` | `CloseEvent` | Conexión cerrada por cualquier parte | `e.code`, `e.reason`, `e.wasClean` |

## `open` — conexión establecida

Se dispara cuando el handshake HTTP→WebSocket concluye con éxito y `readyState` cambia a `OPEN`. A partir de este momento `ws.send()` está disponible.

```js
ws.addEventListener('open', () => {
  ws.send(JSON.stringify({ tipo: 'auth', token: obtenerToken() }));
});
```

## `message` — recibir datos

`e.data` contiene el payload del frame recibido. El tipo depende de si el servidor envió un frame de texto o binario, y del valor de `ws.binaryType`:

| Frame del servidor | `ws.binaryType` | Tipo de `e.data` |
|---|---|---|
| Texto | (cualquiera) | `string` |
| Binario | `"blob"` (defecto) | `Blob` |
| Binario | `"arraybuffer"` | `ArrayBuffer` |

```js
// Recibir JSON (string)
ws.addEventListener('message', (e) => {
  const payload = JSON.parse(e.data);
  despacharAccion(payload);
});

// Recibir binario como ArrayBuffer
ws.binaryType = 'arraybuffer';
ws.addEventListener('message', (e) => {
  if (e.data instanceof ArrayBuffer) {
    procesarBinario(new Uint8Array(e.data));
  } else {
    procesarTexto(e.data);
  }
});
```

## `error` — fallo de conexión

El evento `error` se dispara cuando ocurre un error de red o de protocolo. Por diseño del estándar, el objeto evento es un `Event` genérico sin campo de detalle del error — esto es intencional para no filtrar información de redes internas. Inmediatamente después del `error` se dispara `close`, que sí lleva el código de cierre.

```js
ws.addEventListener('error', () => {
  // No hay información detallada del error aquí
  // El código real llega en el evento 'close' que sigue
  console.warn('Error en WebSocket — esperando evento close');
});
```

## `close` — conexión cerrada

`CloseEvent` lleva tres propiedades útiles:

- `e.code` — entero con el código de cierre RFC 6455. `1000` es cierre limpio; `1006` indica que la conexión se cerró de forma abrupta sin frame de cierre (ej. caída de red).
- `e.reason` — string descriptivo enviado por quien inició el cierre (puede estar vacío).
- `e.wasClean` — `true` si el handshake de cierre completó correctamente; `false` si la conexión se cortó sin cerrar formalmente.

```js
ws.addEventListener('close', (e) => {
  if (!e.wasClean) {
    console.error(`Cierre abrupto — código ${e.code}`);
    intentarReconexion();
  }
});
```

## Reconexión automática con backoff exponencial

El protocolo WebSocket no incluye reconexión automática; si la conexión cae, el objeto `WebSocket` queda en estado `CLOSED` y no se recupera. La reconexión debe implementarse en la aplicación. El patrón estándar usa backoff exponencial con jitter para evitar tormentas de reconexiones simultáneas.

```js
function conectarConBackoff(url, opciones = {}) {
  const {
    maxReintentos = 10,
    baseDelay = 1000,    // ms
    maxDelay = 30_000,   // ms
    onMessage = () => {},
    onOpen = () => {},
  } = opciones;

  let ws;
  let intento = 0;
  let timeoutId = null;

  function conectar() {
    ws = new WebSocket(url);

    ws.addEventListener('open', () => {
      intento = 0; // reset al conectar con éxito
      onOpen(ws);
    });

    ws.addEventListener('message', (e) => onMessage(e.data, ws));

    ws.addEventListener('close', (e) => {
      if (e.wasClean || intento >= maxReintentos) return;

      const delay = Math.min(baseDelay * 2 ** intento, maxDelay);
      const jitter = Math.random() * 0.3 * delay; // ±30%
      intento++;

      console.log(`Reconectando en ${Math.round(delay + jitter)} ms (intento ${intento})`);
      timeoutId = setTimeout(conectar, delay + jitter);
    });
  }

  conectar();

  return {
    enviar(data) {
      if (ws.readyState === WebSocket.OPEN) ws.send(data);
    },
    cerrar() {
      clearTimeout(timeoutId);
      intento = maxReintentos; // evitar reconexión
      ws.close(1000, 'cerrado por la app');
    },
  };
}

// Uso
const cliente = conectarConBackoff('wss://chat.ejemplo.com', {
  onMessage(data) { console.log(JSON.parse(data)); },
  onOpen(ws) { ws.send(JSON.stringify({ tipo: 'auth', token: 'abc123' })); },
});
```

## Chat en tiempo real — enviar y recibir JSON

```js
const ws = new WebSocket('wss://chat.ejemplo.com/sala/42');

// Mostrar mensajes recibidos
ws.addEventListener('message', (e) => {
  const { usuario, texto, timestamp } = JSON.parse(e.data);
  agregarBurbuja(usuario, texto, new Date(timestamp));
});

// Enviar mensaje al pulsar Enter
formulario.addEventListener('submit', (e) => {
  e.preventDefault();
  const texto = inputTexto.value.trim();
  if (!texto || ws.readyState !== WebSocket.OPEN) return;

  ws.send(JSON.stringify({
    tipo: 'mensaje',
    texto,
    timestamp: Date.now(),
  }));

  inputTexto.value = '';
});
```

## Cómo funciona por dentro

Cada evento corresponde a un tipo de frame WebSocket recibido (o a un cambio de estado de la conexión TCP). El evento `open` se dispara cuando el navegador recibe el `101 Switching Protocols`. Cada frame de texto o binario completo genera un evento `message`. Un frame de tipo `close` (opcode `0x8`) — o la rotura de la conexión TCP sin frame de cierre — genera `error` y/o `close`. Los frames `ping` y `pong` del keepalive son transparentes al API JavaScript.

> [!tip]
> Usar `addEventListener` en lugar de `ws.onmessage = fn` cuando se registran múltiples listeners, para evitar que uno sobreescriba al anterior. `ws.onmessage` solo admite un handler; `addEventListener('message', ...)` admite varios independientes.

> [!warning]
> Un `error` seguido de `close` con `e.code === 1006` y `e.wasClean === false` indica caída abrupta de la red o rechazo del servidor sin handshake de cierre. No hay información de causa en el evento — para depurar, revisar la consola de red del navegador (pestaña WS en DevTools).

## Alternativas según el caso de uso

- **`EventSource` / Server-Sent Events** — cuando solo el servidor necesita enviar datos al cliente. Más simple (solo HTTP), reconexión automática incluida en el browser, pero estrictamente unidireccional.
- **`WebTransport`** — basado en QUIC (HTTP/3). Admite streams bidireccionales y datagramas con pérdida tolerable, menor latencia que WebSocket. Menos soporte de navegadores a 2026.

## Notas relacionadas

- [[01 Conexión Persistente|Conexión Persistente]] — `new WebSocket()`, `readyState`, `send()`, `close()`
- [[index|WebSockets — índice de sección]]
