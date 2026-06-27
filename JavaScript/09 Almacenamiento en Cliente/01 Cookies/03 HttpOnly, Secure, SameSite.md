---
title: Cookies — HttpOnly, Secure, SameSite
aliases:
  - HttpOnly
  - cookie Secure
  - SameSite
  - cookie seguridad
tags:
  - javascript
  - api/web
  - almacenamiento
  - seguridad
draft: false
order: 3
---

# HttpOnly, Secure, SameSite

> [!definicion]
> `HttpOnly`, `Secure` y `SameSite` son los atributos de seguridad de las cookies. `HttpOnly` protege contra robo de cookies por XSS (la cookie no es accesible desde JS). `Secure` exige HTTPS para transmitir la cookie. `SameSite` controla el envío en peticiones cross-site y es la defensa principal contra CSRF. En producción, las cookies de sesión deben combinarse los tres.

```js
// Configuración recomendada para cookies de sesión (establecida desde el servidor)
// Set-Cookie: sessionId=abc123; HttpOnly; Secure; SameSite=Lax; Path=/; Max-Age=86400

// Desde el cliente (JS solo puede controlar Secure, SameSite — no HttpOnly)
document.cookie = "preferencia=oscuro; Secure; SameSite=Lax; path=/; max-age=31536000";
```

## `HttpOnly`

Una cookie con `HttpOnly` **no aparece en `document.cookie`** ni es accesible por ninguna API de JS (`cookieStore` tampoco). Solo el navegador la gestiona y la envía en las peticiones HTTP:

```js
// Servidor establece:
// Set-Cookie: sessionId=abc123; HttpOnly; Secure; SameSite=Lax

// En el cliente:
document.cookie; // "preferencia=oscuro"  — sessionId no aparece
```

`HttpOnly` solo puede establecerlo el **servidor** mediante la cabecera `Set-Cookie`. No es posible crear una cookie `HttpOnly` desde `document.cookie` — si se incluye el atributo, el navegador lo ignora y crea la cookie sin él.

**Protección contra XSS:** si un atacante inyecta JS malicioso en la página, `document.cookie` no revela las cookies `HttpOnly`. El token de sesión permanece fuera del alcance del script, incluso en un ataque XSS exitoso.

## `Secure`

La cookie solo se transmite en conexiones **HTTPS**. En HTTP, el navegador omite la cookie de la petición:

```js
// Solo se envía en conexiones cifradas
document.cookie = "token=abc; Secure; SameSite=Lax; path=/; max-age=3600";
```

Sin `Secure`, la cookie se envía tanto en HTTPS como en HTTP — en HTTP un atacante en la red puede interceptarla (ataque man-in-the-middle).

`SameSite=None` requiere obligatoriamente `Secure` en navegadores modernos (Chrome, Firefox). Sin `Secure`, el par `SameSite=None; Secure` no funciona.

## `SameSite`

Controla si la cookie se envía en peticiones **cross-site** (peticiones que se originan desde un dominio diferente al del destino de la cookie):

| Valor | Comportamiento | Cuándo usar |
|---|---|---|
| `Strict` | Solo se envía en peticiones same-site; nunca en navegación desde un enlace externo | Acciones muy sensibles (banca, cambio de contraseña) |
| `Lax` | Default en browsers modernos. Se envía en navegación top-level (clic en enlace) pero no en sub-resources cross-site (fetch, img, iframe) | Sesiones generales; equilibrio seguridad/usabilidad |
| `None` | Siempre se envía, incluidas peticiones cross-site; requiere `Secure` | Cookies de terceros, widgets embebidos, SSO cross-origin |

```js
// SameSite=Strict: máxima protección, puede romper flujos OAuth
document.cookie = "csrfToken=xyz; SameSite=Strict; Secure; path=/";

// SameSite=Lax: default razonable para sesiones
document.cookie = "sesion=abc; SameSite=Lax; Secure; HttpOnly; path=/";

// SameSite=None: cookies de terceros (requiere Secure)
// Solo puede establecerse desde servidor con HttpOnly
// Set-Cookie: tracker=id; SameSite=None; Secure; path=/
```

## SameSite y protección CSRF

El ataque CSRF (Cross-Site Request Forgery) consiste en que un sitio malicioso hace que el navegador de la víctima envíe una petición autenticada al sitio objetivo. `SameSite` corta este vector:

- `SameSite=Strict` — bloquea el envío de la cookie en cualquier petición originada fuera del mismo sitio, incluyendo navegación por enlace.
- `SameSite=Lax` — bloquea el envío en peticiones de sub-resources (fetch, XMLHttpRequest, img, iframe, form con método POST) pero permite el envío en navegación top-level (clic en `<a href>`).
- `SameSite=None` — no ofrece protección CSRF; requiere tokens CSRF adicionales si se usa con datos sensibles.

Con `SameSite=Lax` o `Strict`, un formulario malicioso en `evil.com` no puede usar la cookie de sesión de `example.com`, porque la petición cross-site omite la cookie.

## Tabla resumen de atributos de seguridad

| Atributo | Quién puede establecerlo | Qué protege | Efecto observable |
|---|---|---|---|
| `HttpOnly` | Solo el servidor | XSS — robo de cookies | Cookie invisible en `document.cookie` |
| `Secure` | Servidor o JS | Intercepción en red | Solo se transmite por HTTPS |
| `SameSite=Lax` | Servidor o JS | CSRF | No se envía en peticiones cross-site de sub-resources |
| `SameSite=Strict` | Servidor o JS | CSRF estricto | No se envía en ninguna petición cross-site |
| `SameSite=None` | Solo servidor (con Secure) | Sin protección CSRF | Se envía siempre; requiere Secure |

## Configuración recomendada en producción

```
# Cookie de sesión/autenticación (servidor)
Set-Cookie: sessionId=...; HttpOnly; Secure; SameSite=Lax; Path=/; Max-Age=86400

# Cookie de preferencias de UI (cliente)
document.cookie = "tema=oscuro; Secure; SameSite=Lax; path=/; max-age=31536000";

# Cookie de terceros/SSO (servidor)
Set-Cookie: ssoToken=...; Secure; SameSite=None; Path=/; Max-Age=3600
```

## Cómo funciona por dentro

El algoritmo de envío de cookies del navegador evalúa cada cookie en el jar antes de adjuntarla a una petición. Para `SameSite`, el navegador compara el "site" del contexto de navegación (el origen del documento que inició la petición) con el "site" del destino de la petición, usando el eTLD+1 como unidad de comparación (por eso `app.example.com` y `api.example.com` son "same-site" pero `example.com` y `otherdomain.com` no). Para `Secure`, verifica que la URL de destino use el esquema `https:`. Para `HttpOnly`, simplemente excluye la cookie del getter de `document.cookie` a nivel del motor del navegador.

> [!tip]
> `SameSite=Lax` + `HttpOnly` + `Secure` es la combinación base para cualquier cookie de sesión. Si el flujo de autenticación usa redirecciones OAuth desde dominios externos y la cookie desaparece al volver, considerar cambiar temporalmente a `SameSite=None; Secure` solo para esa cookie o usar tokens en `Authorization` header en lugar de cookies.

> [!warning]
> Navegadores modernos (Chrome 80+, Firefox) aplican `SameSite=Lax` **por defecto** a cookies sin atributo `SameSite`. Una cookie creada sin `SameSite` se comporta como `Lax`, no como `None`. Esto puede romper integraciones cross-origin que asumían el comportamiento antiguo (envío en todas las peticiones).

## Notas relacionadas

- [[01 Leer y Escribir document.cookie|Leer y Escribir document.cookie]] — por qué `document.cookie` no muestra las cookies HttpOnly
- [[02 Atributos (expires, path, domain)|Atributos (expires, path, domain)]] — control de expiración y alcance
- [[index|Cookies]] — visión general y comparativa con Web Storage
