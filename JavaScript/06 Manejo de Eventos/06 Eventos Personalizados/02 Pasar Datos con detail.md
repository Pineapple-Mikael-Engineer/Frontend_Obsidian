---
title: Pasar Datos con detail — CustomEvent.detail
aliases:
  - detail CustomEvent
  - e.detail
  - CustomEvent detail
tags:
  - javascript
  - api/concepto
  - eventos
draft: false
order: 2
---

# Pasar Datos con detail

> [!definicion]
> `detail` es la propiedad de `CustomEvent` destinada a transportar datos arbitrarios del emisor al receptor. Se establece en el constructor: `new CustomEvent('tipo', { detail: datos })`. En el listener se accede como `e.detail`. Puede ser cualquier valor — objeto, array, primitivo — siempre que no se necesite cruzar fronteras de contexto (iframes, workers) donde se requiere serialización.

```js
// Emisor
formulario.dispatchEvent(new CustomEvent('form:enviado', {
  bubbles: true,
  detail: {
    campos: { nombre: 'Ana', email: 'ana@ejemplo.com' },
    timestamp: Date.now(),
  },
}));

// Receptor
document.addEventListener('form:enviado', (e) => {
  console.log(e.detail.campos.nombre);  // 'Ana'
  console.log(e.detail.timestamp);
});
```

## Tipos de valor para `detail`

`detail` puede ser cualquier valor JavaScript:

```js
// Primitivo
new CustomEvent('progreso', { detail: 75 });

// Objeto
new CustomEvent('usuario:login', { detail: { id: 1, rol: 'admin' } });

// Array
new CustomEvent('errores:validacion', { detail: ['Campo requerido', 'Email inválido'] });

// null (ningún dato)
new CustomEvent('modal:cerrado', { bubbles: true, detail: null });
```

## Comunicación componente hijo → padre

Con `bubbles: true`, el hijo emite el evento y el padre lo escucha en su propio elemento. El padre no necesita referencia directa al hijo — el árbol DOM actúa como bus de comunicación.

```js
// Hijo: componente de búsqueda
class BuscadorProductos extends HTMLElement {
  connectedCallback() {
    this.querySelector('input').addEventListener('input', (e) => {
      this.dispatchEvent(new CustomEvent('busqueda:cambio', {
        bubbles: true,           // sube al padre
        composed: true,          // si usa Shadow DOM, cruza la frontera
        detail: { texto: e.target.value },
      }));
    });
  }
}

// Padre: gestor de página
paginaProductos.addEventListener('busqueda:cambio', (e) => {
  filtrarProductos(e.detail.texto);
});
```

El padre (`paginaProductos`) no tiene referencia al `BuscadorProductos`. El desacoplamiento es total.

## Receta: sistema de notificaciones

```js
// Cualquier parte de la app puede emitir una notificación
function notificar(mensaje, tipo = 'info') {
  document.dispatchEvent(new CustomEvent('notificacion:nueva', {
    detail: { mensaje, tipo, id: crypto.randomUUID() },
  }));
}

// El gestor de notificaciones escucha globalmente
document.addEventListener('notificacion:nueva', (e) => {
  const { mensaje, tipo, id } = e.detail;
  mostrarToast(mensaje, tipo, id);
});

// Uso
notificar('Pedido guardado', 'exito');
notificar('Error al conectar con el servidor', 'error');
```

## Comparación con otros patrones

| Patrón | Acoplamiento | Dirección | Requiere referencia |
|---|---|---|---|
| `CustomEvent` + `dispatchEvent` | Bajo | Cualquiera (con burbujeo) | No |
| Callback como prop | Alto | Padre → hijo invierte a hijo→padre | Sí (el padre pasa la función) |
| Pub/Sub manual (EventEmitter) | Bajo | Cualquiera | No (nombre del canal) |
| Estado compartido (store) | Bajo | Cualquiera | No (referencia al store) |

`CustomEvent` es la opción más nativa y sin dependencias — adecuada para componentes web estándar y para comunicación local dentro de una sección del DOM. Para comunicación global a escala de aplicación, un store centralizado o un EventEmitter dedicado puede ser más ergonómico.

> [!warning]
> `detail` no se clona automáticamente — es una referencia al mismo objeto. Si el receptor modifica `e.detail`, modifica el objeto original. Si se necesita inmutabilidad, pasar `Object.freeze(datos)` o una copia: `detail: { ...datos }`.

> [!tip]
> Para establecer un contrato claro entre emisor y receptor, documentar la forma del `detail` en un comentario o JSDoc junto al punto de emisión. Con TypeScript, extender `CustomEvent<T>` para tipado completo: `new CustomEvent<{ id: number }>('pedido:listo', { detail: { id: 5 } })`.

## Notas relacionadas

- [[01 CustomEvent y dispatchEvent|CustomEvent y dispatchEvent]]
- [[06 Eventos Personalizados/index|Eventos Personalizados]]
- [[05 Delegación de Eventos|Delegación de Eventos]]
