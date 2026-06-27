---
title: Propiedades de location y clase URL
aliases:
  - propiedades de location
  - window.location
  - clase URL
  - URL parser
tags:
  - javascript
  - api/objeto
  - browser
draft: false
order: 1
---

# Propiedades de `location` y clase `URL`

> [!definicion]
> `window.location` (también accesible como `location` o `document.location`) es un objeto `Location` que refleja todas las partes de la URL de la página actual. Cada propiedad devuelve la parte correspondiente de la URL como string, y algunas son escribibles (asignar un nuevo valor provoca navegación). La clase `URL` (ES2016) ofrece las mismas propiedades pero para cualquier URL, además de `searchParams` como instancia de `URLSearchParams`.

```js
// URL de ejemplo: https://sub.ejemplo.com:8080/ruta?q=texto#seccion

console.log(location.href);      // "https://sub.ejemplo.com:8080/ruta?q=texto#seccion"
console.log(location.protocol);  // "https:"
console.log(location.host);      // "sub.ejemplo.com:8080"
console.log(location.hostname);  // "sub.ejemplo.com"
console.log(location.port);      // "8080"
console.log(location.pathname);  // "/ruta"
console.log(location.search);    // "?q=texto"
console.log(location.hash);      // "#seccion"
console.log(location.origin);    // "https://sub.ejemplo.com:8080"
```

## Propiedades del objeto `location`

| Propiedad | Parte de la URL | Ejemplo | Escribible |
|---|---|---|---|
| `href` | URL completa | `"https://sub.ejemplo.com:8080/ruta?q=texto#seccion"` | Sí — navega a la nueva URL |
| `protocol` | Esquema con dos puntos | `"https:"` | Sí |
| `host` | Hostname + puerto | `"sub.ejemplo.com:8080"` | Sí |
| `hostname` | Solo el hostname | `"sub.ejemplo.com"` | Sí |
| `port` | Puerto como string | `"8080"` (vacío si es el por defecto) | Sí |
| `pathname` | Ruta, empieza con `/` | `"/ruta"` | Sí |
| `search` | Query string, empieza con `?` | `"?q=texto"` (vacío si no hay) | Sí |
| `hash` | Fragmento, empieza con `#` | `"#seccion"` (vacío si no hay) | Sí |
| `origin` | Protocolo + host | `"https://sub.ejemplo.com:8080"` | No (solo lectura) |

## Navegación asignando propiedades

Asignar a `location.href` es equivalente a llamar a `location.assign(url)`: navega a la nueva URL y añade una entrada al historial. Asignar propiedades individuales como `location.pathname` o `location.search` navega modificando solo esa parte:

```js
// Equivalentes: ambas navegan y añaden al historial
location.href = "https://ejemplo.com/nueva-ruta";
location.assign("https://ejemplo.com/nueva-ruta");

// Modificar solo el hash — no recarga la página
location.hash = "#nueva-seccion";

// Modificar el pathname — sí recarga
location.pathname = "/otra-ruta";

// Modificar el search — sí recarga
location.search = "?filtro=activo";
```

## Clase `URL` — parser completo

La clase `URL` expone las mismas propiedades que `location` pero para cualquier URL (no solo la actual) y además incluye `searchParams` como una instancia viva de `URLSearchParams`:

```js
const url = new URL("https://ejemplo.com/ruta?q=hola&page=2");

url.hostname;                  // "ejemplo.com"
url.pathname;                  // "/ruta"
url.searchParams.get("q");     // "hola"
url.searchParams.get("page");  // "2"

// Las propiedades son escribibles — modifica la URL en memoria
url.pathname = "/otra-ruta";
url.searchParams.set("q", "typescript");
console.log(url.href); // "https://ejemplo.com/otra-ruta?q=typescript&page=2"
```

### Resolución relativa con base URL

`new URL(ruta, base)` resuelve una ruta relativa contra una URL base, igual que lo hace el browser con los atributos `href` de los elementos `<a>`:

```js
const base = "https://ejemplo.com/seccion/pagina";
new URL("../otro", base).href;         // "https://ejemplo.com/otro"
new URL("/raiz", base).href;           // "https://ejemplo.com/raiz"
new URL("?nuevo=param", base).href;   // "https://ejemplo.com/seccion/pagina?nuevo=param"
new URL("#ancla", base).href;          // "https://ejemplo.com/seccion/pagina#ancla"
```

### `URL.canParse()` — validación sin excepciones

`new URL(string)` lanza `TypeError` si la URL es inválida. Para validar sin try/catch, usar el método estático `URL.canParse()` (disponible en browsers modernos desde 2023):

```js
URL.canParse("https://ejemplo.com"); // true
URL.canParse("no-es-una-url");       // false

// Patrón de validación segura
function parsearURL(str) {
  return URL.canParse(str) ? new URL(str) : null;
}
```

## Cómo funciona por dentro

`location` es un objeto host (no un objeto JS puro) gestionado por el motor del browser. Cada vez que se lee una propiedad, el browser parsea la URL de la pestaña actual en ese momento; no hay caché en el lado JS. Al asignar una propiedad escribible, el browser genera una navegación completa (equivalente a que el usuario escribiese la URL en la barra de direcciones), con todo lo que implica: disparo del evento `beforeunload`, entrada en el historial, nueva petición HTTP. La clase `URL`, en cambio, es un objeto JS estándar definido en la especificación WHATWG URL Living Standard — parsea la URL en memoria sin interacción con el motor de navegación.

> [!tip]
> Para extraer partes de la URL actual sin crear una instancia de `URL`, usa directamente las propiedades de `location`. Para construir, manipular o validar URLs arbitrarias (endpoints de API, URLs de redirección, recursos externos), usa `new URL()`: es más seguro que la concatenación de strings y garantiza el encoding correcto de los componentes.

> [!warning]
> `location.port` devuelve un string vacío cuando el puerto es el predeterminado del protocolo (`80` para `http:`, `443` para `https:`), no el número del puerto. Si necesitas el puerto real en todos los casos, usa `url.port || (url.protocol === "https:" ? "443" : "80")`.

## Notas relacionadas

- [[02 reload, assign, replace|reload, assign, replace]] — los métodos de navegación de `location`
- [[03 URLSearchParams|URLSearchParams]] — lectura y modificación ergonómica de `location.search`
- [[../04 History/02 pushState y replaceState|pushState y replaceState]] — cambiar la URL sin recargar ni añadir al historial de forma no deseada
