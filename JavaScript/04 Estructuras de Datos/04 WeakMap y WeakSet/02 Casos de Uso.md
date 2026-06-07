---
title: WeakMap y WeakSet — Casos de Uso
aliases:
  - WeakMap casos de uso
  - WeakSet casos de uso
  - metadatos privados WeakMap
tags:
  - javascript
  - api/clase
  - estructuras-datos
objeto: WeakMap
tipo: patron
draft: false
---

# WeakMap y WeakSet — Casos de Uso

> [!definicion]
> WeakMap y WeakSet son útiles en tres categorías de problemas: (1) **metadatos privados** asociados a objetos sin modificarlos ni impedir su recolección; (2) **caché que se limpia sola** cuando los objetos cacheados ya no existen; (3) **marcado de objetos** para registrar estados de procesamiento sin crear dependencias de ciclo de vida. Son la herramienta correcta cuando los objetos sobre los que trabajas tienen un ciclo de vida que no controlas.

```js
// Patrón central: asociar datos a un objeto sin modificarlo
const _meta = new WeakMap();

class Componente {
  constructor(elemento) {
    _meta.set(this, { elemento, montado: false, eventos: [] });
  }
  montar() {
    const m = _meta.get(this);
    m.montado = true;
    // ...
  }
}
// Cuando la instancia de Componente muere, _meta libera los datos automáticamente
```

## Caso 1: Metadatos privados asociados a objetos

El patrón más frecuente: almacenar información extra sobre un objeto sin añadir propiedades al objeto (evitar contaminar su interfaz) y sin crear retención de memoria.

```js
const metadatos = new WeakMap();

function setMeta(obj, data) {
  metadatos.set(obj, data);
}

function getMeta(obj) {
  return metadatos.get(obj);
}

function incrementarUsos(obj) {
  const meta = metadatos.get(obj) ?? { usos: 0 };
  meta.usos++;
  metadatos.set(obj, meta);
  return meta.usos;
}

const request = {};
setMeta(request, { timestamp: Date.now(), ip: "192.168.1.1" });
incrementarUsos(request);
console.log(getMeta(request)); // → { timestamp: ..., ip: "...", usos: 1 }

// Cuando 'request' salga de alcance, metadatos libera la entrada
```

## Caso 2: Implementación de campos privados pre-ES2022

Antes de que los campos `#privado` llegaran a ES2022, el patrón estándar para privacidad real (no solo convención `_privado`) era usar un WeakMap externo a la clase. Los datos privados no son accesibles desde fuera y se limpian cuando la instancia muere.

```js
const _estado = new WeakMap();

class Contador {
  constructor(inicial = 0) {
    _estado.set(this, { valor: inicial });
  }

  incrementar(n = 1) {
    _estado.get(this).valor += n;
    return this;
  }

  get valor() {
    return _estado.get(this).valor;
  }

  reset() {
    _estado.get(this).valor = 0;
    return this;
  }
}

const c = new Contador(10);
c.incrementar(5).incrementar(3);
console.log(c.valor);         // → 18
console.log(_estado.get(c));  // → { valor: 18 }  (accesible si tienes _estado, como debe ser)

// Con ES2022 esto se hace con #:
class ContadorModerno {
  #valor;
  constructor(inicial = 0) { this.#valor = inicial; }
  get valor() { return this.#valor; }
}
```

## Caso 3: Caché que no previene GC

Cachear resultados de cómputo costoso asociados a objetos que pueden desaparecer — por ejemplo, nodos DOM, instancias de modelos de datos, objetos de terceros.

```js
const cacheResultados = new WeakMap();

function procesarElemento(elemento) {
  if (cacheResultados.has(elemento)) {
    return cacheResultados.get(elemento); // hit
  }

  // Cómputo costoso
  const resultado = { bbox: elemento.getBoundingClientRect(), area: calcularArea(elemento) };
  cacheResultados.set(elemento, resultado);
  return resultado;
}

// Si el elemento es eliminado del DOM y no hay más referencias,
// cacheResultados libera su entrada automáticamente — sin memory leak
```

**Contraste con Map:** si se usara `Map`, habría que limpiar manualmente la caché cuando el objeto se destruye, o el Map lo retendría para siempre.

## Caso 4: WeakSet para marcar objetos procesados

Registrar qué objetos han sido procesados, visitados, o están "en progreso", sin impedir que el GC los limpie cuando ya no son necesarios.

```js
const procesados = new WeakSet();

function procesar(nodo) {
  if (procesados.has(nodo)) return; // ya procesado — saltar
  procesados.add(nodo);

  // lógica de procesamiento
  console.log(`Procesando: ${nodo.id}`);
}

const nodo1 = { id: "a" };
const nodo2 = { id: "b" };

procesar(nodo1); // → "Procesando: a"
procesar(nodo1); // → (silencio — ya procesado)
procesar(nodo2); // → "Procesando: b"
```

### Detectar ciclos en grafos

WeakSet es ideal para algoritmos de recorrido de grafos donde los nodos son objetos:

```js
function recorrerGrafo(nodo, visitados = new WeakSet()) {
  if (visitados.has(nodo)) return; // ciclo detectado — evitar recursión infinita
  visitados.add(nodo);

  console.log(nodo.valor);
  for (const vecino of nodo.vecinos ?? []) {
    recorrerGrafo(vecino, visitados);
  }
}
```

### Marcar elementos DOM en progreso

```js
const enAnimacion = new WeakSet();

function animar(elemento) {
  if (enAnimacion.has(elemento)) return; // ya animando
  enAnimacion.add(elemento);

  elemento.animate([{ opacity: 0 }, { opacity: 1 }], { duration: 300 })
    .addEventListener("finish", () => enAnimacion.delete(elemento));
}
```

## Caso 5: Datos de listener sin retener el target

Asociar metadatos a event listeners o callbacks sin crear retención cruzada:

```js
const listenerMeta = new WeakMap();

function registrarListener(target, handler, meta) {
  listenerMeta.set(handler, meta);
  target.addEventListener("click", handler);
}

function manejarClick(e) {
  const meta = listenerMeta.get(manejarClick);
  console.log(`Click registrado por: ${meta.autor}`);
}

registrarListener(document.getElementById("btn"), manejarClick, { autor: "sistema" });
```

## `WeakRef` y `FinalizationRegistry` (ES2021)

ES2021 añadió primitivos de más bajo nivel para gestión de referencias débiles:

`WeakRef` — una referencia débil explícita a un objeto. El objeto puede ser recolectado; `ref.deref()` devuelve el objeto o `undefined` si ya fue recolectado.

```js
let obj = { dato: 42 };
const ref = new WeakRef(obj);

// En algún momento más tarde:
const vivo = ref.deref();
if (vivo !== undefined) {
  console.log(vivo.dato); // el objeto sigue vivo
} else {
  console.log("Recolectado"); // el GC lo eliminó
}
```

`FinalizationRegistry` — registra un callback que se ejecuta cuando un objeto es recolectado por el GC. Útil para limpieza de recursos externos (handles nativos, workers, sockets).

```js
const registro = new FinalizationRegistry((token) => {
  console.log(`Objeto con token "${token}" fue recolectado`);
  // limpiar recursos externos aquí
});

let recurso = { handle: abrirRecursoNativo() };
registro.register(recurso, "recurso-1");

recurso = null; // cuando el GC lo recoja, el callback se ejecutará
```

> [!tip] Buenas prácticas
> - Usar WeakMap para metadatos privados cuando los objetos tienen ciclos de vida impredecibles (DOM, instancias de framework, objetos de terceros).
> - Preferir campos `#` (ES2022) sobre el patrón WeakMap para privacidad en clases propias — es más claro y con el mismo resultado.
> - Usar WeakSet para marcar objetos durante algoritmos que recorren estructuras de datos (grafos, árboles) — la memoria se libera automáticamente cuando el algoritmo termina.
> - `WeakRef` y `FinalizationRegistry` son para casos muy avanzados — la mayoría de código no los necesita.

> [!warning] Errores comunes
> - **Usar WeakMap como almacén general:** sin iteración ni `size`, no es posible enumerar las entradas. Para almacenamiento general, usar Map.
> - **Esperar limpieza inmediata:** el GC no es síncrono. La entrada de WeakMap desaparece en el próximo ciclo de GC, no al instante de asignar `null`.
> - **Usar `FinalizationRegistry` para lógica de negocio crítica:** los callbacks de finalización no están garantizados en todos los entornos ni en todos los ciclos.
> - **Pasar primitivos a WeakSet/WeakMap:** `ws.add(42)` lanza `TypeError` — solo objetos.

## Notas relacionadas

- [[04 WeakMap y WeakSet/index|WeakMap y WeakSet — índice]]
- [[01 Claves Débiles y Garbage Collection]]
- [[02 Map/index|Map — índice]]
- [[03 Set/index|Set — índice]]
