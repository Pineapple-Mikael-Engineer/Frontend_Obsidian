---
title: document.cookie — leer y escribir cookies desde JS
aliases:
  - document.cookie
  - leer cookies JS
  - escribir cookies JS
tags:
  - javascript
  - api/propiedad
  - almacenamiento
objeto: document
tipo: propiedad
draft: false
order: 1
---

# Leer y Escribir `document.cookie`

> [!definicion]
> `document.cookie` es una propiedad string con una interfaz no intuitiva: al **leer** devuelve todas las cookies visibles para el documento como un string `"nombre1=valor1; nombre2=valor2"` (sin atributos), y al **escribir** no sobreescribe todas las cookies sino que **añade o actualiza** una sola cookie. Solo las cookies sin `HttpOnly` son accesibles desde JS.

```js
// Leer todas las cookies visibles
console.log(document.cookie);
// "sessionId=abc123; tema=oscuro; idioma=es"

// Escribir (añadir/actualizar) una cookie
document.cookie = "tema=claro; max-age=86400; path=/; SameSite=Lax";

// La asignación NO borra las otras cookies
console.log(document.cookie);
// "sessionId=abc123; tema=claro; idioma=es"
```

## La interfaz string y su parseo

Al leer `document.cookie` se obtiene un string plano sin atributos (`expires`, `path`, `SameSite`, etc.). Para acceder a una cookie individual hay que parsear manualmente:

```js
function getCookie(nombre) {
  return document.cookie
    .split("; ")
    .find(c => c.startsWith(nombre + "="))
    ?.split("=")[1] ?? null;
}

getCookie("tema"); // "claro"
getCookie("no-existe"); // null
```

El método `find` localiza la primera coincidencia. Usar `startsWith(nombre + "=")` en lugar de solo `nombre` evita falsos positivos con nombres que son prefijo de otro (`"id"` vs `"idUsuario"`).

## Helpers completos: setCookie, getCookie, deleteCookie

```js
function setCookie(nombre, valor, opciones = {}) {
  const {
    maxAge,
    expires,
    path = "/",
    domain,
    secure = false,
    sameSite = "Lax",
  } = opciones;

  let cookie = `${encodeURIComponent(nombre)}=${encodeURIComponent(valor)}`;

  if (maxAge !== undefined) cookie += `; max-age=${maxAge}`;
  if (expires)              cookie += `; expires=${expires.toUTCString()}`;
  if (path)                 cookie += `; path=${path}`;
  if (domain)               cookie += `; domain=${domain}`;
  if (secure)               cookie += "; Secure";
  cookie += `; SameSite=${sameSite}`;

  document.cookie = cookie;
}

function getCookie(nombre) {
  const clave = encodeURIComponent(nombre) + "=";
  return document.cookie
    .split("; ")
    .find(c => c.startsWith(clave))
    ?.slice(clave.length)
    .split(";")[0]  // por si hay caracteres inesperados
    ?? null;
}

function deleteCookie(nombre, path = "/") {
  document.cookie = `${encodeURIComponent(nombre)}=; max-age=0; path=${path}`;
}
```

## Borrar una cookie

Para eliminar una cookie desde JS hay que establecerla con `max-age=0` o con `expires` en el pasado. El nombre y el `path` deben coincidir exactamente con los de la cookie original:

```js
// Borrar inmediatamente
document.cookie = "tema=; max-age=0; path=/";

// Alternativa con expires en el pasado
document.cookie = "tema=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/";
```

## Codificación de valores

Los valores de las cookies no permiten ciertos caracteres (punto y coma, coma, espacio). Usar `encodeURIComponent` al escribir y `decodeURIComponent` al leer garantiza compatibilidad:

```js
setCookie("busqueda", "zapatos rojos talla 42");
// Almacena: busqueda=zapatos%20rojos%20talla%2042

const valor = decodeURIComponent(getCookie("busqueda") ?? "");
// "zapatos rojos talla 42"
```

## API moderna: CookieStore (experimental)

`CookieStore` es una API basada en Promises disponible en navegadores modernos (Chrome 87+) y en Service Workers — a diferencia de `document.cookie`:

```js
// Leer una cookie
const cookie = await cookieStore.get("tema");
console.log(cookie?.value); // "claro"

// Leer todas las cookies
const todas = await cookieStore.getAll();
console.log(todas); // [{name: "tema", value: "claro", ...}, ...]

// Escribir
await cookieStore.set({
  name: "tema",
  value: "oscuro",
  maxAge: 86400,
  path: "/",
  sameSite: "lax",
});

// Eliminar
await cookieStore.delete("tema");

// Escuchar cambios
cookieStore.addEventListener("change", e => {
  console.log("Cambiadas:", e.changed);
  console.log("Eliminadas:", e.deleted);
});
```

| Característica | `document.cookie` | `CookieStore` |
|---|---|---|
| Interfaz | Síncrona, string | Asíncrona, Promises |
| Acceso individual | Parseo manual | `get(nombre)` directo |
| Disponible en SW | No | Sí |
| Atributos al leer | No devuelve | Sí devuelve |
| Soporte | Universal | Chrome 87+, Edge 87+ |

## Cómo funciona por dentro

`document.cookie` es una propiedad con getters/setters especiales implementados en el motor del navegador. El getter consulta el jar de cookies del origen actual y construye el string en el momento de la lectura. El setter llama internamente al mismo algoritmo que usa el navegador para procesar la cabecera `Set-Cookie` de las respuestas HTTP, por lo que respeta las mismas reglas de validación de atributos.

Las cookies `HttpOnly` son gestionadas exclusivamente por la capa HTTP del navegador y se omiten del getter de `document.cookie` a nivel del motor, no por el script.

> [!tip]
> Cuando se trabaja con muchas cookies, cachear `document.cookie.split("; ")` en una variable local en lugar de llamar al getter en un bucle evita múltiples accesos a la capa nativa. El getter construye el string cada vez que se accede.

> [!warning]
> `document.cookie` no está disponible en Service Workers (el contexto `self` no tiene `document`). Para acceder a cookies desde un SW hay que usar `CookieStore` o leer las cookies de las `Request` que intercepta el SW mediante `request.headers.get("cookie")`.

## Notas relacionadas

- [[02 Atributos (expires, path, domain)|Atributos (expires, path, domain)]] — cuándo y dónde se envía la cookie
- [[03 HttpOnly, Secure, SameSite|HttpOnly, Secure, SameSite]] — por qué `HttpOnly` oculta la cookie de `document.cookie`
- [[index|Cookies]] — introducción y comparativa con Web Storage
