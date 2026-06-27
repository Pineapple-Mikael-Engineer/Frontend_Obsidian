---
title: URLSearchParams
aliases:
  - URLSearchParams
  - query string
  - query params
tags:
  - javascript
  - api/objeto
  - browser
draft: false
order: 3
---

# `URLSearchParams`

> [!definicion]
> `URLSearchParams` es la interfaz del navegador para leer, escribir y serializar los parámetros de consulta (query string) de una URL de forma ergonómica. Evita la manipulación manual de strings o regex para parsear `?clave=valor&otra=dato`. Soporta claves duplicadas (varios valores para la misma clave) y se encarga automáticamente del encoding/decoding de los caracteres especiales.

```js
const params = new URLSearchParams("?q=javascript&page=2&page=3");

params.get("q");       // "javascript"
params.getAll("page"); // ["2", "3"]
params.has("q");       // true
params.has("x");       // false

params.set("q", "typescript"); // reemplaza todos los valores de "q"
params.append("tag", "web");   // añade sin borrar valores existentes
params.delete("page");         // elimina todos los valores de "page"

params.toString(); // "q=typescript&tag=web"
```

## Métodos de `URLSearchParams`

| Método / Propiedad | Descripción |
|---|---|
| `get(key)` | Primer valor de la clave, o `null` si no existe |
| `getAll(key)` | Array con todos los valores de la clave (vacío si no existe) |
| `has(key)` | `true` si la clave existe |
| `has(key, value)` | `true` si existe el par clave-valor exacto |
| `set(key, value)` | Establece el valor, eliminando duplicados previos de esa clave |
| `append(key, value)` | Añade el valor sin eliminar los existentes (permite duplicados) |
| `delete(key)` | Elimina todos los valores de la clave |
| `delete(key, value)` | Elimina solo la ocurrencia del par exacto |
| `toString()` | Serializa a string de query (`"clave=valor&otra=dato"`, sin el `?`) |
| `entries()` | Iterador de pares `[clave, valor]` |
| `keys()` | Iterador de claves |
| `values()` | Iterador de valores |
| `forEach(fn)` | Itera con callback `(valor, clave, params)` |
| `sort()` | Ordena los parámetros por clave (en orden Unicode) |
| `size` | Número total de pares clave-valor |

## Formas de construir una instancia

```js
// Desde un string (con o sin el "?" inicial)
const p1 = new URLSearchParams("q=hola&page=1");
const p2 = new URLSearchParams("?q=hola&page=1"); // también funciona

// Desde un objeto plano — cada clave se convierte a string
const p3 = new URLSearchParams({ q: "hola", page: 1, activo: true });
p3.toString(); // "q=hola&page=1&activo=true"

// Desde un array de pares [clave, valor]
const p4 = new URLSearchParams([["q", "hola"], ["tag", "web"], ["tag", "js"]]);
p4.getAll("tag"); // ["web", "js"]

// Desde la URL actual (forma moderna)
const p5 = new URL(window.location.href).searchParams;
// O más directo, disponible en browsers modernos:
const p6 = new URLSearchParams(location.search);
```

## Leer los parámetros de la URL actual

```js
// URL actual: /buscar?q=typescript&categoria=frontend&page=2

const params = new URLSearchParams(location.search);
// Equivalente con clase URL:
const params2 = new URL(location.href).searchParams;

const consulta   = params.get("q");         // "typescript"
const categoria  = params.get("categoria"); // "frontend"
const pagina     = Number(params.get("page") ?? "1"); // 2
const inexistente = params.get("otro");     // null
```

## Actualizar la URL sin recargar

Para sincronizar los parámetros de la URL con el estado de la interfaz (filtros, búsqueda, paginación) sin recargar la página, combinar `URLSearchParams` con `history.pushState` o `history.replaceState`:

```js
function actualizarFiltros(filtros) {
  const params = new URLSearchParams(location.search);

  // Sincronizar cada filtro
  Object.entries(filtros).forEach(([clave, valor]) => {
    if (valor) {
      params.set(clave, String(valor));
    } else {
      params.delete(clave);
    }
  });

  // replaceState: no queremos que cada cambio de filtro cree una entrada en el historial
  const nuevaURL = `${location.pathname}?${params.toString()}`;
  history.replaceState({}, "", nuevaURL);
}

// Uso
actualizarFiltros({ categoria: "frontend", page: 3, descuento: "" });
// URL: /buscar?q=typescript&categoria=frontend&page=3 (descuento eliminado)
```

## Receta: filtros de búsqueda sincronizados con la URL

Patrón completo para sincronizar la UI con la URL, de modo que los filtros sean compartibles por URL y se restauren al cargar la página:

```js
// Al cargar la página: restaurar el estado de la UI desde la URL
function restaurarFiltrosDesdeURL() {
  const params = new URLSearchParams(location.search);

  document.getElementById("busqueda").value = params.get("q") ?? "";
  document.getElementById("categoria").value = params.get("cat") ?? "todas";
  document.getElementById("pagina").textContent = params.get("page") ?? "1";
}

// Al cambiar un filtro: actualizar la URL y volver a buscar
function aplicarFiltro(clave, valor) {
  const params = new URLSearchParams(location.search);
  if (valor) {
    params.set(clave, valor);
  } else {
    params.delete(clave);
  }
  params.delete("page"); // al cambiar filtros, volver a la página 1

  history.pushState({}, "", `?${params.toString()}`);
  buscar(params); // función que lanza la petición con los filtros
}

// Al navegar con atrás/adelante: restaurar desde el nuevo location
window.addEventListener("popstate", () => {
  restaurarFiltrosDesdeURL();
  buscar(new URLSearchParams(location.search));
});

// Inicialización
restaurarFiltrosDesdeURL();
buscar(new URLSearchParams(location.search));
```

## Encoding automático

`URLSearchParams` codifica y decodifica los caracteres especiales automáticamente (percent-encoding). No es necesario llamar a `encodeURIComponent` manualmente:

```js
const params = new URLSearchParams();
params.set("q", "hello world & más");
params.toString(); // "q=hello+world+%26+m%C3%A1s"
// Los espacios se codifican como "+" (application/x-www-form-urlencoded)

// Al leer, la decodificación es automática
params.get("q"); // "hello world & más"
```

## Cómo funciona por dentro

`URLSearchParams` implementa el formato `application/x-www-form-urlencoded` definido en la especificación WHATWG URL. Internamente mantiene una lista ordenada de pares `[nombre, valor]` (no un mapa, de ahí que permita claves duplicadas). Los espacios se codifican como `+` (no como `%20` como haría `encodeURIComponent`). Cuando se obtiene la instancia `searchParams` de un objeto `URL`, esa instancia está vinculada al objeto `URL` — modificar los params actualiza automáticamente `url.href`, y modificar `url.search` actualiza automáticamente los params.

> [!tip]
> `new URL(location.href).searchParams` devuelve una instancia de `URLSearchParams` vinculada al objeto `URL`, lo que permite modificar params y leer el href actualizado directamente. Sin embargo, para solo leer los params de la URL actual, `new URLSearchParams(location.search)` es más directo y eficiente.

> [!warning]
> `URLSearchParams.toString()` no incluye el `?` inicial. Al construir la URL final para `history.pushState` o para asignar a `location.search`, añadirlo explícitamente: `` `?${params.toString()}` ``. Asignar `location.search = params.toString()` (sin `?`) funciona en la mayoría de browsers porque lo añaden automáticamente, pero no es fiable en todos los entornos — mejor ser explícito.

## Notas relacionadas

- [[01 Propiedades de location|Propiedades de location]] — `location.search` y la clase `URL`
- [[../04 History/02 pushState y replaceState|pushState y replaceState]] — actualizar la URL con los nuevos params sin recargar
- [[../04 History/03 Evento popstate|Evento popstate]] — restaurar el estado de filtros al navegar atrás/adelante
