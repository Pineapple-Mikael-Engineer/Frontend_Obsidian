---
title: Verificación de Sitio — Probar la propiedad del dominio
aliases:
  - site verification
  - verificación de sitio
tags:
  - html
  - api/concepto
  - metadatos
draft: false
order: 3
---

# Verificación de Sitio

> [!definicion]
> La verificación de sitio demuestra a un servicio (Google Search Console, Bing Webmaster, Pinterest…) que **eres el dueño** del dominio, para acceder a sus herramientas. Uno de los métodos es colocar una etiqueta `<meta>` con un código que el servicio te proporciona.

```html
<meta name="google-site-verification" content="código-único-proporcionado" />
```

## Métodos de verificación

| Método | Cómo |
|--------|------|
| Etiqueta `<meta>` | Añadir el `<meta>` de verificación al `<head>` |
| Archivo HTML | Subir un archivo concreto a la raíz del sitio |
| Registro DNS | Añadir un registro TXT al DNS del dominio |
| Google Analytics / Tag Manager | Si ya están instalados |

La etiqueta `<meta>` es el método más sencillo cuando se controla el HTML de la página.

## Etiquetas por servicio

```html
<meta name="google-site-verification" content="…" />
<meta name="msvalidate.01" content="…" />            <!-- Bing -->
<meta name="p:domain_verify" content="…" />          <!-- Pinterest -->
<meta name="yandex-verification" content="…" />      <!-- Yandex -->
```

## Para qué sirve verificar

> [!info] Acceso a las herramientas para webmasters
> Verificar el sitio en Google Search Console da acceso a datos valiosísimos: qué consultas traen tráfico, qué páginas están indexadas, errores de rastreo, posición media, Core Web Vitals, y la posibilidad de enviar el sitemap. Es uno de los primeros pasos al lanzar un sitio.

## Buenas prácticas

> [!tip] Recomendaciones
> - Verifica el sitio en Search Console nada más lanzarlo.
> - No borres la etiqueta de verificación tras verificar: si desaparece, puedes perder el acceso.
> - El método DNS verifica el dominio entero (todos los subdominios y protocolos); el `<meta>`, solo esa propiedad.

## Errores comunes

> [!warning] Trampas
> - **Quitar la etiqueta** tras verificar: algunos servicios revocan el acceso.
> - **Copiar mal el código**: la verificación falla silenciosamente.
> - **Confundir verificación con indexación**: verificar no hace que Google indexe; solo da acceso a las herramientas.

## Notas relacionadas

- [[02 Robots (meta robots)]] — otro `<meta>` para buscadores.
- [[index]] — el resto de metadatos estándar.
