---
title: <meta http-equiv> — Refresh y redirección
aliases:
  - meta refresh
  - http-equiv
tags:
  - html
  - api/elemento
  - metadatos
elemento: meta
categoria: metadatos
rol_implicito: ninguno
vacio: true
draft: false
order: 4
---

# Refresh y Redirección (http-equiv)

> [!definicion]
> `<meta http-equiv>` simula cabeceras HTTP desde el HTML. El uso más conocido, `refresh`, **recarga o redirige** la página tras unos segundos. Funciona, pero está **desaconsejado** frente a las redirecciones reales del servidor.

```html
<!-- Redirige a otra página tras 5 segundos -->
<meta http-equiv="refresh" content="5; url=https://nuevo-sitio.com" />

<!-- Recarga la misma página cada 30 segundos -->
<meta http-equiv="refresh" content="30" />
```

## Sintaxis de content

| `content` | Efecto |
|-----------|--------|
| `5` | Recarga la página a los 5 segundos |
| `0; url=...` | Redirige inmediatamente a esa URL |
| `10; url=...` | Redirige a los 10 segundos |

## Por qué evitar meta refresh para redirigir

> [!warning] Usa una redirección 301/302 del servidor
> Redirigir con `meta refresh` tiene problemas serios:
> - **SEO**: no transmite la autoridad de la página antigua como sí lo hace una redirección 301; Google lo trata peor.
> - **Accesibilidad**: una redirección temporizada puede desorientar; un refresh automático que recarga viola WCAG (2.2.1) si el usuario no puede controlarlo.
> - **Usabilidad**: rompe el botón "atrás" (vuelve a la página que redirige, en bucle).
>
> La forma correcta de redirigir es una **respuesta HTTP 301** (permanente) o **302** (temporal) desde el servidor. `meta refresh` solo es un último recurso cuando no se tiene acceso a la configuración del servidor.

## Otros usos de http-equiv

| `http-equiv` | Para qué |
|--------------|----------|
| `refresh` | Recargar/redirigir (desaconsejado) |
| `content-security-policy` | Definir una CSP desde el HTML (mejor por cabecera) |
| `x-ua-compatible` | Modo de compatibilidad de IE (legado) |

## Buenas prácticas

> [!tip] Recomendaciones
> - Para redirigir, usa **301/302 del servidor**, no `meta refresh`.
> - Evita recargas automáticas con `refresh`; si las necesitas, hazlo con JavaScript controlable.
> - Define la CSP por cabecera HTTP cuando sea posible, no por `http-equiv`.

## Errores comunes

> [!warning] Trampas
> - **`meta refresh` para redirecciones permanentes**: perjudica el SEO y la usabilidad.
> - **Auto-refresh sin control del usuario**: problema de accesibilidad.
> - **Romper el botón atrás** con redirecciones temporizadas.

## Notas relacionadas

- [[11 Enlace Canónico (link rel canonical)]] — la forma correcta de gestionar URLs duplicadas.
- [[index]] — el resto de metadatos estándar.
