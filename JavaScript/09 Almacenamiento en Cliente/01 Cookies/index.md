---
title: Cookies — almacenamiento HTTP persistente
aliases:
  - Cookies
  - HTTP cookies
tags:
  - javascript
  - api/web
  - almacenamiento
draft: false
---

# Cookies

> [!definicion]
> Las **cookies** son pequeños fragmentos de datos que el servidor o el cliente pueden almacenar en el navegador y que el navegador envía automáticamente en cada petición HTTP al dominio correspondiente. Esta transmisión automática las hace ideales para sesiones y autenticación, pero también las convierte en vectores de ataque (XSS, CSRF). Límite: ~4KB por cookie, ~50 cookies por dominio.

```js
// Escribir una cookie desde el cliente
document.cookie = "usuario=ana; max-age=3600; path=/; SameSite=Lax";

// Leer todas las cookies visibles
console.log(document.cookie); // "usuario=ana; tema=oscuro"
```

A diferencia de `localStorage`, las cookies tienen fecha de expiración configurable, se envían al servidor con cada petición y pueden marcarse como `HttpOnly` para que JavaScript no pueda acceder a ellas. Esta combinación las convierte en el mecanismo estándar para tokens de sesión gestionados desde el backend.

## Bloques de esta sección

- [[01 Leer y Escribir document.cookie|Leer y Escribir document.cookie]] — la API string de `document.cookie`, parseo manual, helpers y CookieStore.
- [[02 Atributos (expires, path, domain)|Atributos (expires, path, domain)]] — control de expiración, alcance de ruta y dominio.
- [[03 HttpOnly, Secure, SameSite|HttpOnly, Secure, SameSite]] — atributos de seguridad, protección contra XSS y CSRF.

## Comparativa rápida con Web Storage

| Característica | Cookies | localStorage | sessionStorage |
|---|---|---|---|
| Límite de tamaño | ~4KB | ~5-10MB | ~5-10MB |
| Enviada al servidor | Sí, automáticamente | No | No |
| Expiración | Configurable | Sin expiración | Al cerrar la pestaña |
| Accesible desde JS | Solo sin HttpOnly | Sí | Sí |
| Disponible en SW | No | No | No |

## Cuándo usar cookies

Usar cookies cuando el servidor necesita conocer el estado del cliente (sesiones, autenticación, preferencias regionales). Para almacenamiento puramente en el cliente que el servidor no necesita leer, [[../02 Web Storage/index|Web Storage]] es la alternativa con mayor capacidad y sin el overhead de transmisión.

> [!tip]
> En producción, las cookies de sesión y autenticación deben ser establecidas por el servidor con `HttpOnly; Secure; SameSite=Lax` para maximizar la protección. Las cookies escritas desde JS son útiles principalmente para preferencias no críticas.

> [!warning]
> Las cookies comparten el límite por dominio (~50) entre todas las pestañas y origen. Si una aplicación necesita almacenar muchos datos en el cliente, `localStorage` o `IndexedDB` son alternativas con mayor capacidad.

## Notas relacionadas

- [[../02 Web Storage/index|Web Storage]] — localStorage y sessionStorage como alternativa sin transmisión al servidor
- [[../03 IndexedDB/index|IndexedDB]] — almacenamiento estructurado para grandes volúmenes de datos
