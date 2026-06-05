---
title: <link rel="stylesheet"> — Enlace a hoja de estilos externa
aliases:
  - link element
  - stylesheet link
tags:
  - html
  - api/elemento
  - metadatos
elemento: link
categoria: metadatos
rol_implicito: ninguno
vacio: true
draft: false
---

# Enlace a CSS (link)

> [!definicion]
> `<link rel="stylesheet" href="…">` vincula una **hoja de estilos externa** al documento. Es la
> forma recomendada de aplicar CSS: separa presentación de estructura y permite **cachear** el `.css`
> para reutilizarlo entre páginas. El elemento `<link>` es void (sin cierre) y es mucho más versátil
> que solo CSS: su comportamiento lo define el atributo `rel`.

```html
<head>
  <link rel="stylesheet" href="/css/estilos.css" />
</head>
```

## Atributos

| Atributo | Valores | Descripción |
|----------|---------|-------------|
| `rel` | `stylesheet`, `icon`, `canonical`, `preload`, `preconnect`… | **Relación** del recurso |
| `href` | URL | Ruta al recurso enlazado |
| `media` | `screen`, `print`, `(max-width: 600px)` | Condición para aplicar la hoja |
| `crossorigin` | `anonymous`, `use-credentials` | Política CORS para recursos de otro origen |
| `integrity` | hash SHA | Verifica que el recurso no fue alterado (SRI) |

## Un elemento, muchos usos según rel

El mismo `<link>` sirve para propósitos muy distintos. Conviene verlos juntos porque se confunden:

| `rel` | Función |
|-------|---------|
| `stylesheet` | Hoja de estilos (esta nota) |
| `icon` | Favicon de la pestaña |
| `canonical` | URL oficial de la página |
| `preload` | Descargar un recurso crítico cuanto antes |
| `preconnect` | Abrir conexión anticipada a otro origen |
| `alternate` | Versión alternativa (RSS, otro idioma) |

El favicon se desarrolla en [[10 Favicon (link rel icon)]] y la URL oficial en
[[11 Enlace Canónico (link rel canonical)]].

## Carga condicional con media

Una hoja puede aplicarse solo bajo cierta condición. El navegador la descarga con **baja prioridad**
si la condición no se cumple, sin bloquear el render:

```html
<!-- Solo al imprimir -->
<link rel="stylesheet" href="print.css" media="print" />

<!-- Solo en pantallas anchas -->
<link rel="stylesheet" href="desktop.css" media="(min-width: 1024px)" />
```

## Rendimiento: el CSS bloquea el render

> [!info] Render-blocking, y está bien que lo sea
> Las hojas en el `<head>` son **render-blocking**: el navegador no pinta hasta tenerlas y aplicarlas.
> Esto evita el *flash de contenido sin estilo* (FOUC), donde el usuario ve un instante de HTML
> desnudo. La contrapartida: una hoja pesada retrasa el primer pintado. Por eso el CSS va arriba (en
> el `<head>`) y el JavaScript pesado abajo o con `defer`.

### preload para CSS crítico

Para acelerar, se puede precargar la hoja clave con alta prioridad:

```html
<link rel="preload" href="critico.css" as="style" />
<link rel="stylesheet" href="critico.css" />
```

## Buenas prácticas

> [!tip] Recomendaciones
> - **CSS externo** en producción: cacheable y compartido entre páginas, mejor que
>   [[08 Estilos Internos (style) | `<style>`]] embebido.
> - Carga las hojas no críticas (impresión, temas alternativos) con `media`.
> - Para recursos de CDN externos, añade `integrity` + `crossorigin` (Subresource Integrity).

## Errores comunes

> [!warning] Trampas
> - **Olvidar `rel="stylesheet"`**: sin él, el navegador no aplica el CSS (un `<link href="x.css">`
>   suelto no hace nada).
> - **Demasiadas hojas separadas**: cada `<link>` es una petición; en HTTP/1 conviene agrupar, aunque
>   con HTTP/2 el coste baja.
> - **Ruta relativa mal calculada**: `href` se resuelve respecto a la URL del documento (o a `<base>`
>   si existe).

## Notas relacionadas

- [[08 Estilos Internos (style)]] — CSS embebido, alternativa para casos puntuales.
- [[09 Scripts (script)]] — el equivalente para JavaScript.
- [[10 Favicon (link rel icon)]] — el mismo elemento `<link>` con otro `rel`.
