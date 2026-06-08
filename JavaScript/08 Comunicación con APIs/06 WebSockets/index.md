---
title: WebSockets — comunicación bidireccional en tiempo real
aliases:
  - WebSocket
  - WebSockets
  - ws
tags:
  - javascript
  - api/clase
  - red
draft: false
---

# WebSockets

> [!definicion]
> **WebSocket** es un protocolo de comunicación full-duplex sobre una única conexión TCP persistente. A diferencia de HTTP (modelo request-response), la conexión permanece abierta y tanto el cliente como el servidor pueden enviar mensajes en cualquier momento, sin que el otro lado lo solicite. El API del navegador se accede a través de la clase `WebSocket`.

El handshake inicial es HTTP (con cabecera `Upgrade: websocket`), lo que permite que la conexión pase por proxies y firewalls que solo admiten HTTP/HTTPS. Tras el handshake, el protocolo cambia a frames WebSocket binarios, más eficientes que HTTP para mensajes frecuentes y de pequeño tamaño.

```js
const ws = new WebSocket('wss://chat.ejemplo.com/sala/general');

ws.addEventListener('open', () => ws.send(JSON.stringify({ tipo: 'hola' })));
ws.addEventListener('message', (e) => console.log(JSON.parse(e.data)));
ws.addEventListener('close', (e) => console.log(`Cerrado: ${e.code} ${e.reason}`));
```

## Bloques de esta sección

- [[01 Conexión Persistente|Conexión Persistente]] — `new WebSocket()`, `readyState`, `send()`, `close()`, `bufferedAmount`, `binaryType`.
- [[02 Eventos (open, message, error, close)|Eventos (open, message, error, close)]] — ciclo de vida de la conexión, `MessageEvent`, `CloseEvent`, reconexión automática con backoff exponencial.

## Comparativa con otras tecnologías de tiempo real

| Tecnología | Dirección | Protocolo base | Caso de uso típico |
|---|---|---|---|
| WebSocket | Bidireccional | TCP (upgrade HTTP) | Chat, juegos, feeds financieros |
| Server-Sent Events | Solo servidor→cliente | HTTP (stream) | Notificaciones, feeds de noticias |
| Long Polling | Solo servidor→cliente | HTTP | Compatibilidad con servidores legados |
| WebTransport | Bidireccional | QUIC (HTTP/3) | Baja latencia, pérdida tolerable |
| Fetch / XHR | Solo cliente→servidor (request) | HTTP | APIs REST, one-shot requests |

## Casos de uso

WebSocket resulta apropiado cuando la latencia importa y los mensajes fluyen en ambas direcciones con frecuencia:

- **Chat en tiempo real** — mensajes, presencia, escritura activa.
- **Feeds financieros** — precios de acciones o criptomonedas que actualizan múltiples veces por segundo.
- **Juegos multijugador** — sincronización de estado de partida.
- **Dashboards de monitoreo** — métricas de servidores, logs en vivo.
- **Colaboración en documentos** — edición simultánea (al estilo de Google Docs).
- **Notificaciones push** — cuando `EventSource` no es suficiente por necesitar envío bidireccional.

## Notas relacionadas

- [[01 Fetch API/index|Fetch API]] — peticiones HTTP one-shot, el modelo opuesto a WebSocket
- [[05 CORS/index|CORS]] — el handshake WebSocket incluye cabeceras de origen sujetas a CORS
