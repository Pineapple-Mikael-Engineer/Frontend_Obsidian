---
title: Cookies — atributos expires, max-age, path, domain
aliases:
  - cookie expires
  - cookie max-age
  - cookie path
  - cookie domain
  - atributos de cookie
tags:
  - javascript
  - api/web
  - almacenamiento
draft: false
---

# Atributos de Cookie: expires, path, domain

> [!definicion]
> Los atributos de una cookie controlan **cuándo** expira y **dónde** se envía. Se añaden al string de `document.cookie` separados por punto y coma tras el par `nombre=valor`. Sin `expires` ni `max-age`, la cookie es de **sesión** — el navegador la elimina al cerrarse.

```js
// Cookie con expiración explícita, restringida a /app y al dominio raíz
document.cookie = "sesion=xyz; max-age=3600; path=/app; domain=example.com; SameSite=Lax";
```

## Tabla de atributos de alcance y expiración

| Atributo | Valor | Efecto | Ejemplo |
|---|---|---|---|
| `expires` | Fecha en formato GMT | Expiración absoluta; sin este ni `max-age`, la cookie es de sesión | `expires=Fri, 31 Dec 2026 23:59:59 GMT` |
| `max-age` | Número entero (segundos) | Expiración relativa desde ahora; tiene **precedencia** sobre `expires` | `max-age=3600` (1 hora) |
| `path` | Ruta URL | La cookie se envía solo en peticiones a esa ruta y sus descendientes | `path=/dashboard` |
| `domain` | Nombre de dominio | La cookie se envía al dominio y sus subdominios | `domain=example.com` |

## `expires` — expiración absoluta

`expires` recibe una fecha en formato UTC/GMT. Sin este atributo, la cookie dura hasta que se cierra el navegador:

```js
const fecha = new Date();
fecha.setDate(fecha.getDate() + 30); // 30 días desde ahora

document.cookie = `recordarme=token123; expires=${fecha.toUTCString()}; path=/`;
// expires=Thu, 08 Jul 2026 10:00:00 GMT
```

`toUTCString()` genera el formato correcto. No usar `toISOString()` — no es el formato que espera la especificación de cookies.

## `max-age` — expiración relativa

`max-age` acepta segundos y tiene **precedencia sobre `expires`** cuando ambos están presentes. Es más conveniente que `expires` porque no requiere construir un objeto `Date`:

```js
// 1 hora
document.cookie = "temp=abc; max-age=3600; path=/";

// 30 días
document.cookie = "pref=oscuro; max-age=2592000; path=/";

// Borrar inmediatamente (max-age=0 o negativo)
document.cookie = "temp=; max-age=0; path=/";
```

| Duración | max-age |
|---|---|
| 1 hora | 3 600 |
| 1 día | 86 400 |
| 7 días | 604 800 |
| 30 días | 2 592 000 |
| 1 año | 31 536 000 |

## `path` — alcance de ruta

La cookie solo se incluye en las peticiones cuya URL empieza con el `path` especificado:

```js
// Solo se envía en /admin y /admin/*
document.cookie = "adminToken=xyz; path=/admin; max-age=3600";

// Se envía en todas las rutas (recomendado por defecto)
document.cookie = "tema=oscuro; path=/; max-age=31536000";
```

Sin `path`, el valor por defecto es la **ruta del documento actual** — si la página está en `/app/config`, la cookie solo se envía en peticiones a `/app/config` y subdirectorios, no a `/` ni a `/app`. Especificar `path=/` explícitamente en casi todos los casos.

## `domain` — alcance de dominio

Sin `domain`, la cookie pertenece exclusivamente al **host exacto** que la creó. Con `domain` especificado, se envía también a subdominios:

```js
// Sin domain: solo para app.example.com
document.cookie = "local=val; path=/";

// Con domain: para example.com, app.example.com, api.example.com, etc.
document.cookie = "compartida=val; domain=example.com; path=/";
```

No es posible establecer una cookie para un dominio distinto al del documento actual ni para un dominio padre más arriba del eTLD+1 (no se puede establecer una cookie para `.com`).

## Cookie de sesión vs persistente

| Tipo | Configuración | Comportamiento |
|---|---|---|
| Sesión | Sin `expires` ni `max-age` | El navegador la elimina al cerrarse (comportamiento exacto varía: algunos browsers restauran sesiones) |
| Persistente | Con `expires` o `max-age` | Sobrevive al cierre del navegador hasta la fecha indicada |

> [!warning]
> "Cookie de sesión" no garantiza que se borre al cerrar la pestaña en todos los navegadores. Chrome, por ejemplo, puede restaurar sesiones y con ellas las cookies de sesión. Para datos temporales que deben desaparecer al cerrar la pestaña, `sessionStorage` es más predecible.

## Cómo funciona por dentro

Cuando el navegador envía una petición HTTP, examina el jar de cookies del perfil activo y adjunta en la cabecera `Cookie` todos los pares `nombre=valor` cuyo `domain` coincide con el host de destino y cuyo `path` es prefijo de la ruta solicitada. Los atributos (`expires`, `path`, `domain`, `Secure`, `SameSite`) nunca se transmiten al servidor — solo se envía el par nombre-valor. El servidor no puede conocer los atributos de una cookie leyendo la cabecera `Cookie`.

> [!tip]
> Para una cookie de preferencias de UI que debe funcionar en toda la aplicación, `path=/` y `max-age` de 1 año es la configuración estándar. Evitar omitir `path` para no depender de la ruta del documento en el momento de creación.

## Notas relacionadas

- [[01 Leer y Escribir document.cookie|Leer y Escribir document.cookie]] — la API para crear cookies con estos atributos
- [[03 HttpOnly, Secure, SameSite|HttpOnly, Secure, SameSite]] — atributos de seguridad que complementan a estos
- [[index|Cookies]] — visión general y límites
